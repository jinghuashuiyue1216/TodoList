<template>
  <div id="app">
    <div class="container">
     <div class="container-wrap">
      <TodoHeader :addItem="addItem"></TodoHeader>
      <TodoList :list="list" :checkItem="checkItem" :deleteItem="deleteItem" :updateItem="updateItem" ></TodoList>
      <TodoFooter :list="list" :deleteAllItem="deleteAllItem" :selectAll="selectAll" :notSelectAll="notSelectAll"></TodoFooter>
     </div>
    </div>
  </div>
</template>
<script>
   import 'ant-design-vue/dist/antd';
   import { message } from 'ant-design-vue';
   import TodoHeader from './components/TodoHeader.vue';
   import TodoList from './components/TodoList.vue';
   import TodoFooter from './components/TodoFooter.vue';
   export default{
    name: "app",
    components:{
    TodoHeader,
    TodoList,
    TodoFooter,
},
    data() {
    return {
      list: [],//由于这个数组三个组件都在用，所以放在App中，状态提升
    };
  }, 
  methods: {
    addItem(itemObj) {  //往数组的前方添加对象
      this.list.unshift(itemObj);
      this.save();
    },
    checkItem(id) {  //拿到id遍历数组找到对应的对象修改状态
      this.list.map((itemObj)=>{
        if(itemObj.id==id) itemObj.isdone=!itemObj.isdone;
      })
      this.save();
    },
    deleteItem(id){  //通过id删除对应的对象
      console.log(id);
      this.list=this.list.filter((itemObj)=>itemObj.id!=id);
      this.save();
    },
    updateItem(id,text){  //拿到id遍历数组找到对应的对象修改内容
      console.log(id);
      this.list.map((itemObj)=>{
        if(itemObj.id==id) item.text=text;
      })
      this.save();
    },
    save() {//执行添加、删除操作后也要用localstorage来存储到本地
      localStorage.list = JSON.stringify(this.list);
    },
    deleteAllItem(){
      this.list=this.list.filter((item)=>{return !item.isdone});
      this.save();
      message.success('已删除');
    },
    selectAll(){ //全选
      this.list.map((itemObj)=>{
        if(itemObj.isdone===false)
        itemObj.isdone=true;
        })
    },
    notSelectAll(){//全不选
      this.list.map((itemObj)=>{
        itemObj.isdone=false;
     })
    }
  },
  created() {//在created钩子函数中初始化json数据
    let list = localStorage.list;
    if (list) {
      this.list = JSON.parse(list);
    }
  }

   }
</script>
<style scoped>
.container{
  width: 600px;
  margin: 0 auto;
}
.container-wrap {
  background-color: rgba(229, 242, 252, 0.8);
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  border-color: rgba(82, 168, 236, 0.8);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075),
    0 0 8px rgba(82, 168, 236, 0.6);
}
.header a-input{
  width: 560px;
  height: 30px;
  font-size: 14px;
}
.header{
  margin-bottom: 10px;
}
.item{
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-left:5px;
  padding-right:5px;
}
.item:hover {
  background-color: rgba(140, 205, 255, 0.8);
}
.left{
  display: flex;
  align-items: center;
}
.content{
  margin-left:15px;
}
.footer{
  display: flex;
  justify-content: space-between;
  text-align: center;
  padding-left:5px;
  padding-right:5px;
  padding-top: 10px;
}

/* input{
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 4px 7px;
}
 input:focus {
  outline: none;
  border-color: rgba(82, 168, 236, 0.8);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075),
    0 0 8px rgba(82, 168, 236, 0.6);
} */
/* li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
} */
/* .btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2),
    0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}

.btn-danger {
  color: #fff;
  background-color: #fcac62;
  border: 1px solid #fa9333;
}

.btn-danger:hover {
  color: #fff;
  background-color: #ff8b1e;
}

.btn:focus {
  outline: none;
} */

/* li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
} */

/* li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}

li:hover {
  background-color: rgba(140, 205, 255, 0.8);
}
li:hover button {
  display: block;
} */

</style>
   
