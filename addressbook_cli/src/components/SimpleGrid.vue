<template id="grid-template">
  <div>
    <table>
      <thead>
        <tr>
          <th>名字</th>
          <th>电话号</th>
          <th>性别</th>
          <th>删除</th>
        </tr>
      </thead>
      <tbody>
        <!--
        <tr v-for="(index,entry) in dataList | filterBy searchKey">
        -->
        <tr v-for="(people, index) in filterdPeople" :key="index">
          <td v-for="(col, index) in columns" :key="index">
            {{ people[col.name] }}
          </td>
          <td class="text-center">
            <button class="btn-delete" @click="deleteItem(people)">删除</button>
          </td>
        </tr>
      </tbody>
    </table>
    <div class="container">
      <button v-on:click="openNewItemDialog('添加新联系人')">添加联系人</button>
    </div>

    <modal-dialog
      :mode="mode"
      :title="title"
      :fields="columns"
      :item="item"
      :dialog-show="dlgShow"
      v-on:create-item="createItem"
      v-on:close-dialog="closeDialog"
    >
    </modal-dialog>
  </div>
</template>

<script>
import ModalDialog from './ModalDialog.vue'
import axios from 'axios'

export default {
  name: "GridTemplate",
  components: {
    "modal-dialog": ModalDialog
  },
  data: function () {
    return {
      mode: 0,
      title: "",
      item: {},
      dlgShow: false,
      creatPeopleUrl: "http://211.149.193.19:8080/api/creatPeople",
      removePeopleUrl: "http://211.149.193.19:8080/api/removePeople",
    };
  },
  props: ["dataList", "columns", "searchKey"],
  created: function () {
    //this.$root.eventHub.$on('show-dialog', this.showDialog)
    //let vm = this;
  },
  methods: {
    openNewItemDialog(title) {
      // 对话框的标题
      this.title = title;
      // mode = 1表示新建模式
      this.mode = 1;
      // 初始化this.item
      this.item = {};
      // 广播事件，showDialog是modal-dialog组件的一个方法，传入参数true表示显示对话框
      //this.$broadcast('showDialog', true)
      //this.$emit("show-dialog", true);
      this.dlgShow = true;
    },
    createItem: function () {
      // 将item追加到dataList
      if (this.item.name!=null && this.item.telephone!=null && this.item.gender!=null)
      {
        this.dataList.push(this.item);
      }

      // 创建联系人到服务端
      //this.createPeople(); // 回调函数里面的this 是什么和下面的区别
      // 广播事件，传入参数false表示隐藏对话框
      //this.$broadcast('showDialog', false)
      //this.$root.eventHub.$emit("show-dialog", false);
      this.dlgShow = false;
      // 新建完数据后，重置item对象
      this.item = {};
    },
    closeDialog: function(){
      this.dlgShow = false;
      this.item = {};
    },
    deleteItem: function (people) {
      var data = this.dataList;
      data.forEach(function (item, i) {
        if (item === people) {
          data.splice(i, 1);
          return;
        }
      });
      this.dlgShow = false;
      //从服务端删除
      // var resource = this.$resource(this.removePeopleUrl);
      // let vm = this;

      // resource.remove({ id: customer.customerId }).then((response) => {
      //   //vm.getCustomers()
      // });
    },
    createPeople: function () {
      var resource = this.$resource(this.creatPeopleUrl);
      vm = this;
      resource.save(vm.creatPeopleUrl, vm.item).then((response) => {
        //vm.$set('item', {})
        //vm.getCustomers()
      });
    },
  },
  computed: {
    filterdPeople: function () {
      var self = this;
      return this.dataList.filter(function (dataList) {
        return dataList.name.indexOf(self.searchKey) !== -1;
      });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
	font-family: Helvetica, simhei, Arial, sans-serif;
}

html {
	font-size: 1rem;
}

body{
	margin-top: 100px;
}

table,
td,
th {
	border-collapse: collapse;/*collapse 如果可能，边框会合并为一个单一的边框。会忽略 border-spacing 和 empty-cells 属性。*/
	border-spacing: 0
}

table {
	width: 100%;
}

td,
th {
	border: 1px solid #bcbcbc;
	padding: 5px 10px;
}

th {
	padding: 10px;
	font-weight: 400;
	color: #fff;
	background: #0090d3;
	cursor: pointer;
}
/*设置表格间隔样式*/
tr:nth-of-type(odd) {
	background: #fff
}

tr:nth-of-type(even) {
	background: #eee
}

h1{
	font-size: 1.5rem;
	margin-bottom: 2rem;
}

input {
	outline: none
}

input[type=text] {
	padding: 3px 6px;
	font-size: 1.2rem;
	border: 1px solid #ccc;
	transition: .3s ease-out;
}

input[type=text]:focus {
	border-color: #0090d3;
	transition: .3s ease-in;
}

button {
	display: inline-block;
	box-sizing: border-box;
	padding: 10px 30px;/*上内边距和下内边距是 10px右内边距和左内边距是 30px*/
	background: #0090d3;
	color: #fff;
	border: 1px solid #0090d3;
	border-radius: 3px;	
	outline: 0;

	transition: .4s ease-out;
}

button:hover,
button:focus {
	opacity: 0.8;
	cursor: pointer;
	transition: .15s ease-in;
}

#app {
	margin: 0 auto;
	max-width: 640px;
}

.btn-delete{
	padding: 5px 15px;
	border: 1px solid salmon;
	background: salmon;
}

.container {
	padding-left: 15px;
	padding-right: 15px;
	margin: 10px;
}

.search-input {
	width: 80%;
}

.form-group {
	margin: 10px;
}

.form-group > label {
	display: inline-block;
	padding-right: 1rem;
	width: 5rem;
	text-align: right;
}

.form-group > input,
.form-group > select {
	display: inline-block;
	height: 1.8rem;
	line-height: 1.8rem;
}

.text-center {
	text-align: center;
}
</style>
