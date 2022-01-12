<template>
  <div id="app">
    <div class="container">
      <div class="form-group">
        <label>搜索</label>
        <input type="text" class="search-input" v-model="searchQuery" />
      </div>
    </div>
    <div class="container">
      <simple-grid
        :data-list="people"
        :columns="columns"
        :search-key="searchQuery"
      >
      </simple-grid>
    </div>
  </div>
</template>

<script>
import SimpleGrid from './components/SimpleGrid.vue'
import axios from 'axios'

export default {
  name: "App",
  components: {
    "simple-grid": SimpleGrid
  },
  data: function () {
    return {
    //eventHub: new Vue(),
    searchQuery: "",
    columns: [
      {
        name: "name",
        isKey: true,
      },
      {
        name: "telephone",
      },
      {
        name: "gender",
        dataSource: ["男", "女"],
      },
    ],
    people: [],
    // peopleTmp: [
    //   {
    //     name: "秦国海",
    //     telephone: 1231231,
    //     gender: "男",
    //   },
    //   {
    //     name: "赵孝军",
    //     telephone: 12313,
    //     gender: "男",
    //   },
    // ],
    apiUrl: "http://211.149.193.19:8080/api/getPepples",
    apiUrlTest: '/getPepples',
    apiUrlTest1: 'http://127.0.0.1:8090'
  }
},
  created () {
    //this.people = this.peopleTmp
    // 加载通讯录时所有人
    //this.getPeoples();
    axios.get(this.apiUrlTest).then((response)=>{
        if (response.data == undefined)
        {
          return
        }
        //this.people = response.data
        var arr = response.data
        for (let i in arr)
        //for (let i = 0; i < arr.length; i++)
        {
          var person = new Object();
          person.id = arr[i].id;
          person.name = arr[i].name;
          person.telephone =arr[i].telephone;
          person.gender = arr[i].gender;
          //var person = new Object();
          // person.id = arr[i].id;
          // person.name = arr[i].name;
          // person.telephone = arr[i].telephone;
          // person.gender = arr[i].gender;
          // var person = new Object();
          // person.id = 1;
          // person.name = "qwe";
          // person.telephone = 1233;
          // person.gender = "男";
          this.people.push(person);
        }

      }).catch((response)=>{
        console.log(response);
      })
  },
  methods: {
    getPeoples: function () {
      axios.get(apiUrlTest).then((response)=>{
        this.people = response
      }).catch((response)=>{
        console.log(response);
      })
      // var resource = this.$resource(this.apiUrl);
      // let vm = this;

      // resource
      //   .get()
      //   .then((response) => {
      //     //vm.$set("people", response.data);
      //     people = response.data
      //   })
      //   .catch(function (response) {
      //     console.log(response);
      //   });
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
