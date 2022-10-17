<template>
    <div class="header">
        <h4 class="logo">今日待办：</h4>
        <a-input type="text"  v-model="inputstr" @keydown.enter="add" placeholder="输入代办事项,按回车添加" />
    </div>
</template>
<script>
// 引入nanoid生成唯一字符串给id
  import { nanoid } from "nanoid";
  export default{
    name:"TodoHeader",
    props:["addItem"],
    data(){
       return{
        inputstr: "",
       }
    },
    methods:{
      add() {
      if(!this.inputstr.trim()) return alert("输入不能为空");
      const itemObj={id:nanoid(), text: this.inputstr,isdone: false};  //根据输入框输入的内容生成一个item对象
      this.addItem(itemObj);//调用父组件里的添加对象的方法
      this.inputstr = "";//将该字符串置为空
      }
    }
  }
</script>
