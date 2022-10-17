<template>
    <a-list-item class="item">          
      <div class="left">
        <a-checkbox :checked="item.isdone" @click="handleCheck(item.id)"></a-checkbox>
        <div class="content">
          <div @dblclick="updateIndex()" v-if="ifnum==-1">{{item.text}}</div>
          <a-input type="text" v-if="ifnum==1" v-model="item.text" @blur="complete(item.id,item.text)"/> 
        </div>            
        </div>
        <a-popconfirm title="你确定要删除吗？" ok-text="Yes" cancel-text="No" @confirm="handleDelete(item.id)"> 
          <a-button type="primary" danger >删除</a-button>
        </a-popconfirm>
    </a-list-item>
</template>
<script>
import { message } from 'ant-design-vue';

export default{
    name:"TodoItem",
    data(){
        return{
          ifnum: -1, 
        }
    },
    props:["item","checkItem","deleteItem","updateItem"],
    methods:{
      handleCheck(id){  
         this.checkItem(id);
      },
      handleDelete(id){
        this.deleteItem(id);
        message.success('已删除');
      },
      updateIndex() {//双击以后隐藏span,显示input
      this.ifnum =1;
    },
     complete(id,text) {//修改完成后点击空白处，隐藏input,显示span
      this.ifnum = -1;
      this.updateItem(id,text);
     },
    }
}
</script>
<style scoped>
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
</style>