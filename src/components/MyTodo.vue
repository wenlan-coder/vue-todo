<template>
  <div id="my-todo" v-show="itemTodo" >
  <!-- <input type="checkbox" v-model="checkall"> -->
  <input type="checkbox" :checked="isall" @change="checkAll">
  <span>已经完成{{donelist}}/全部完成{{all}}</span>
  <button @click="deleteDone">清楚已完成</button>

  </div>
</template>

<script>

export default {
  props:['todo','deleteAll'],

  computed:{
    itemTodo(){
      return this.todo.length!==0

    },
    isall(){
      return this.donelist===this.all&&this.all>0

    },
    // checkall:{
    //   get(){
    //      return this.donelist===this.all&&this.all>0
    //   },
    //   set(val){

    //     this.selectAll(val)
    //   }
    // },
    all(){
      return this.todo.length
    },
    // donelist(){
    //   let i =0;
    //   this.todo.forEach(element => {
    //     if(element.done===true)   i++;
     
    //   });
    //   return i


    // }
    donelist(){ 
      return this.todo.reduce((pre,cur)=>{
        return pre + (cur.done===true ? 1 : 0)
      },0)}
  },
  methods:{
    checkAll(e){
      console.log(e.target.checked); 
      this.$emit('selectAll',e.target.checked)
      
    },
    deleteDone(){
      // console.log("click deletedone");
      this.deleteAll()
      
    }
  }
  

}
</script>

<style scoped lang="less">
#my-todo{
  position: relative;
  width: 600px;
  margin: 0 auto;
  line-height: 50px;
  height: 50px;
  border: 1px
  solid #fff;
  button{

    position: absolute;
    right: 20px;
    top:25px;
    transform: translateY(-50%);
    background:blueviolet;
    border: 1px solid red;
    border-radius: 5%;
    color: white;
    transition: all 0.3s;
    display: none;
   }
}
#my-todo:hover{
  background: #fff;
  button{
    display: block;

  }
}
</style>