<template>
<div id="my-input">
    <div class="todo">
        <span>Todo List</span>
    <input type="text" placeholder="请输入代办事项           crtl +/"  
    @keyup.enter="add"
    required
    ref="list"> </div> 
</div>
  
</template>

<script>
import {nanoid} from 'nanoid'
export default {
    props:['additem'],
    methods:{
         shortcut(){
      console.log("按下快捷键");
    },
        add(e){
            if(e.target.value==''){
                alert('输入不能为空');
               
            }
            else{
                 console.log("clcik enter");
            // console.log(nanoid());
            // console.log(e.target.value);
            //包装成对象
            const obj = {id:nanoid(),title:e.target.value,done:false};
            console.log(obj);
            this.additem(obj)
            e.target.value=""
            }
           
            
            
        }
    }
    

}
</script>

<style scoped lang="less">
#my-input{
  display: flex;
  height: 50px;
  color: aliceblue;
  align-items: center;
  background: #333;
  
  .todo{
      width: 600px;
      margin: 0 auto;
      display: flex;
      justify-content: space-around;
    
  }
}


</style>