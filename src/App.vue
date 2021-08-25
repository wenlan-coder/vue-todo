<template>
  <div id="bg-body">
    <div id="app">
      <my-input :additem="additem" ref="myInput"></my-input>
      <my-item :todo="todo"></my-item>
      <my-todo
        :todo="todo"
        @selectAll="selectAll"
        :deleteAll="deleteAll"
      ></my-todo>
    </div>
  </div>
</template>

<script>
//第三方库写上面
import pubsub from 'pubsub-js'
import MyInput from "./components/MyInput.vue";
import MyItem from "./components/MyItem.vue";
import MyTodo from "./components/MyTodo.vue";
export default {
  name: "App",
  data(){
    return {
      todo:JSON.parse(localStorage.getItem("todos")) || []

    }
  },
  components: {
    MyInput,
    MyItem,
    MyTodo,
  },
  mounted() {
    // console.log(this.$refs.myInput);
     document.onkeydown=e=>{
      //  console.log(e.keyCode);
       let key = e.keyCode;
       if(key==191&&event.ctrlKey) {
        //  alert("按下ctrl+/")
         window.event.preventDefault() //关闭浏览器快捷键
         this.focusInput()   //回调
       };
     }
     
    // console.log(JSON.parse(localStorage.getItem('todos')));
    // this.todo.unshift({ id: 11, title: "test", done: false }),
      this.$bus.$on("deleteTodo", this.deleteTodo),
      this.$bus.$on('updateTitle',this.updateTitle)
      // this.$bus.$on("checkTodo", this.checkTodo);
      //消息订阅
      pubsub.subscribe('checkTodo',this.checkTodo)
  },
  beforeDestroy(){
    this.$bus.$off('deleteTodo'),
    // this.$bus.$off('checkTodo')
    //取消订阅
    pubsub.unsubscribe('checkTodo')

  },
  //add
  methods: {
    //shortcut
    focusInput(){
     let ele=this.$refs.myInput.$refs.list;
     ele.focus()

    },
    additem(item) {
      this.todo.unshift(item)
      console.log("app组件里面",item); 
    },
    //多选或单选  消息订阅传入还有一个订阅名称 msg 用_占位
    checkTodo(_,id) {
      this.todo.forEach((val) => {
        if (val.id === id) val.done = !val.done;
      });
    },
    //select all
    selectAll(item) {
      this.todo.forEach((ele) => {
        ele.done = item;
      });
    },
    //delete todo
    deleteTodo(id) {
      // console.log("app组件里面",id);
      this.todo = this.todo.filter((val) => {
        return val.id !== id;
      });
    },
    //delete all
    deleteAll() {
      if(confirm('确定要清空吗')){
         this.todo = this.todo.filter((item) => {
        return !item.done;
      });
      }     
    },
    //更新todo
    updateTitle(id,title){
      // console.log('app组件里面更新edit');
      this.todo.forEach((val)=>{
        if(val.id===id) {val.title=title}
      })
    },
  },
  watch: {
    todo: {
      handler(val) {
        localStorage.setItem("todos",JSON.stringify(val));
      },
      deep:true,
    },
  },
};
</script>

<style>
#bg-body {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow-y: hidden;
  background: #cdcdcd;
  text-align: center;
}
#app {
  margin: 0 auto;
}
</style>
