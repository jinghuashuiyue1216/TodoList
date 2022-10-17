<template>
      <a-list size="small">
        <TodoItem
         v-for="item in list" 
         :key="item.id" 
         :checkItem="checkItem"
         :deleteItem="deleteItem" 
         :updateItem="updateItem"
         :item="item">
        </TodoItem>
      </a-list>
</template>
<script>
   import TodoItem from "./TodoItem.vue";
   export default{
    name:"TodoList",
    components:{
       TodoItem, 
    },
    props:["list","checkItem","deleteItem","updateItem"],
   };
</script>