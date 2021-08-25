<template>
<transition name="hello" appear="">
      <div class="list" >
      <!-- 不推荐修改了props v-model-->
      <!-- <input type="checkbox" v-model="item.done"> -->
      <input type="checkbox" :checked=item.done @change="handleTodo(item.id)">
      <div v-show="!item.isEdit">{{item.title}}</div>
      <input type="text" 
      v-show="item.isEdit"
      :value="item.title" 
      class="edit-input" 
      @blur="hideEdit($event,item)"
      ref="inputTitle">
      <button class="btn-edit" @click="editTodo(item)" v-show=!item.isEdit>编辑</button>
      <button @click="deleteSingle(item.id)"  class="btn-danger">删除</button>
  </div>
</transition>
</template>

<script>
import pubsub from 'pubsub-js'
export default {
  // data(){
  //   return{
  //     isEdit:false
  //   }
  // },
  props:['item','deleteTodo'],
  methods:{
    deleteSingle(id){
      if(confirm("确定要删除吗"))
    //  this.deleteTodo(id)
    this.$bus.$emit('deleteTodo',id)

    },
    handleTodo(id){
      console.log(id);
      // this.$bus.$emit('checkTodo',id)
      //消息发布
      pubsub.publish('checkTodo',id)

    },
    editTodo(item){
      // console.log(item);
      // console.log('app组件里',item);
      if(item.hasOwnProperty('isEdit')){
        // console.log('已经存在isedit');
          item.isEdit =true
      }else{
        //不存在则重新添加
        // console.log('初次编辑');
         this.$set(item,'isEdit',true)
      }
    
      //vue不会立刻解析模板，二是执行所以的再解析 
      // setTimeout(()=>{
      //   this.$refs.inputTitle.focus()
      // },0)
      //使用nextclick
      this.$nextTick(()=>{
        this.$refs.inputTitle.focus()

      })
   
    },
    hideEdit(e,item){
      // console.log(item.id);
      // console.log(e.target.value);
      item.isEdit=false;
      this.$bus.$emit('updateTitle',item.id,e.target.value)
    }

  },
          
  data(){
    return {

    }
  }

}
</script>

<style lang="less" scoped>
  .hello-enter-active{
            animation: wenlan 1s;

        }
  .hello-leave-active{
            animation: wenlan 1s reverse;

        }
    @keyframes wenlan{
            from{
                transform:translateX(100%);
            }
            to{
                transform:translateX(0)
            }
        }
.list{
  width: 600px;
  border: 1px solid #fff;
  display:inline-flex;
  position: relative;
  height: 30px;
  align-items: center;
  justify-content: space-between;
  div{
    position: absolute;
    left: 20px;
  }
  .edit-input{
    position: absolute;
    left: 20px;
  }
 
  button{
    border: 1px solid silver;
  }
  .btn-edit{
    background: skyblue;
    display:none;
   position: absolute;
   right:70px;

  }
 .btn-danger{
   background: pink;
   display:none;
   position: absolute;
   right: 20px;
 }
}
.list:hover{
  background: #eee;

}
.list:hover button{
   display: block;
}

</style>