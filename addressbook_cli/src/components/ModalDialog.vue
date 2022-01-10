<template id="grid-template">
  <div class="dialogs">
    <div class="dialog" v-bind:class="{ 'dialog-active': dialogShow }">
      <div class="dialog-content">
        <header class="dialog-header">
          <h1 class="dialog-title">{{ title }}</h1>
        </header>
        <div class="dialog-body">
          <div v-for="(field, index) in fields" class="form-group" :key="index">
            <label>{{ field.name }}</label>
            <select v-if="field.dataSource" v-model="item[field.name]">
              <option
                v-for="(opt, index) in field.dataSource"
                :value="opt"
                :key="index"
              >
                {{ opt }}
              </option>
            </select>
            <input v-else type="text" v-model="item[field.name]" />
          </div>
        </div>
        <footer class="dialog-footer">
          <div class="form-group">
            <label></label>
            <button class="btn-save" v-on:click="save">保存</button>
            <button class="btn-close" v-on:click="close">关闭</button>
          </div>
        </footer>
      </div>
    </div>
    <div class="dialog-overlay"></div>
  </div>
</template>

<script>
export default {
  name: "ModalDialog",
  data: function () {
    return {
      // 对话框默认是不显示的
      //show: false,
    };
  },
  /*
   * mode = 1是新增数据模式，mode = 2是修改数据模式
   * title表示对话框的标题内容
   * fields表示对话框要显示的数据字段数组
   * item是由simple-grid传下来，用于绑定表单字段的
   */
  props: ["mode", "title", "fields", "item", "dialogShow"],

  created: function () {
    //this.$root.eventHub.$on('show-dialog', this.showDialog)
    // this.$on("show-dialog", (data) => {
    //   console.log("接受到的参数：" + data);
    //   this.dialogShow= data;
    // });
  },

  methods: {
    close: function () {
      this.dialogShow = false;
    },
    save: function () {
      if (this.mode === 1) {
        //新建模式
        this.$emit("create-item");
      }
    }
  }
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

.btn-save{
	border: 1px solid #0090d3;
	background: #0090d3;
}

.btn-close{
	border: 1px solid #ccc;
	background: #ccc;
}

.text-center {
	text-align: center;
}

.dialog {
	width: 480px;
	position: fixed;
	left: 50%;
	top: 2em;
	transform: translateX(-50%);
	z-index: 2000; /*设置堆叠顺序置顶*/
	visibility: hidden; /**/
	backface-visibility: hidden;
	perspective: 1300px;
}

.dialog-active {
	visibility: visible;
}

.dialog-active .dialog-content {
	opacity: 1;
	transform: rotateY(0);
}

/*overflow 属性规定当内容溢出元素框时发生的事情*/
.dialog-active ~ .dialog-overlay {
	opacity: 1;
	visibility: hidden;
}

.dialog-content {
	border-radius: 3px;
	background: #fff;
	overflow: hidden; /*overflow 属性规定当内容溢出元素框时发生的事情*/
	box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
	transition: .5s ease-in-out;
	opacity: 0;
	transform-style: preserve-3d;/*transform-style 属性规定如何在 3D 空间中呈现被嵌套的元素。preserve-3d 子元素将保留其 3D 位置。
	*/
	transform: rotateY(-670deg);
}

.dialog-header {
	background: #0090d3;
	color: #fff;
}

.dialog-title {
	margin: 0;
	font-size: 2em;
	text-align: center;
	font-weight: 200;
	line-height: 2em;
}

.dialog-body {
	padding: 2em;
}

.dialog-footer {
	margin: 0 2em;
	padding: 1em 0;
	border-top: 1px solid rgba(0, 0, 0, 0.1);
}

.dialog-overlay {
	content: "";
	position: fixed;
	visibility: hidden;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	z-index: 1000;
	opacity: 0;
	background: rgba(0, 0, 0, 0.5);
	transition: all .6s;
}
</style>
