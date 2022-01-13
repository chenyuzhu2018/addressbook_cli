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
        v-on:delete-people="deletePeople"
        v-on:create-people="createPeople"
      >
      </simple-grid>
    </div>
  </div>
</template>

<script>
import SimpleGrid from '@/components/SimpleGrid.vue'
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
    apiUrlGetPepples: '/getPepples',
    apiUrlRemovePepple: '/removePepple',
    apiUrlAddPepple: '/addPeople'
  }
},
  created () {
    // 加载通讯录时所有人
    this.getPeoples();
  },
  methods: {
    getPeoples: function () {

      axios.get(this.apiUrlGetPepples).then((response)=>{
        if (response.data == undefined)
        {
          return
        }
        this.people = []
        var arr = response.data
        for (let i in arr)
        {
          var person = new Object();
          person.id = arr[i].id;
          person.name = arr[i].name;
          person.telephone =arr[i].telephone;
          person.gender = arr[i].gender
          this.people.push(person);
        }

      }).catch((response)=>{
        console.log(response);
      })
    },
    deletePeople: function (people) {
      let removePeppleUrl = this.apiUrlRemovePepple + '/' + people.id
      let vm = this
      axios.get(removePeppleUrl).then((response)=>{
        vm.getPeoples()
      }).catch((response)=>{
        console.log(response);
      })
      
      //var data = this.people;
      // data.forEach(function (item, i) {
      //   if (item === people) {
      //     data.splice(i, 1);
      //     return;
      //   }
      // });
    },
    createPeople: function (people) {
      let vm = this
      let jsonBody = {};
      jsonBody.id = -1
      jsonBody.name = people.name
      jsonBody.telephone = people.telephone
      jsonBody.gender = people.gender
      axios.post(this.apiUrlAddPepple, jsonBody).then((response)=>{
        vm.getPeoples()
      }).catch((response)=>{
        console.log(response);
      })
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
