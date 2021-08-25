<template>
  <div id="my-item">
  <list :item="item" 
  v-for="item in todo" 
  :key="item.id"
  ></list>
  </div>
</template>

<script>
import List from "./List.vue"
export default {
    props:["todo","deleteTodo"],
    components:{
        List,
    },


}
</script>

<style scoped>
#my-item{
  width: 600px;
  margin: 0 auto;

}

</style>