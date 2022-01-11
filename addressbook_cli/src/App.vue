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
        name: "名字",
        isKey: true,
      },
      {
        name: "电话号",
      },
      {
        name: "性别",
        dataSource: ["男", "女"],
      },
    ],
    people: [],
    // peopleTmp: [
    //   {
    //     名字: "秦国海",
    //     电话号: 1231231,
    //     性别: "男",
    //   },
    //   {
    //     名字: "赵孝军",
    //     电话号: 12313,
    //     性别: "男",
    //   },
    // ],
    apiUrl: "http://211.149.193.19:8080/api/getPepples",
    apiUrlTest: '/baidu',
    apiUrlTest1: 'https://www.baidu.com'
  }
},
  created () {
    // 加载通讯录时所有人
    //this.getPeoples();
    axios.get(this.apiUrlTest).then((response)=>{
        this.people = response
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
