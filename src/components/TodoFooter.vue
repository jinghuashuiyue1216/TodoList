<template>
  <div class="footer">
    <div class="left">
      <a-checkbox :checked="flag" @click="handleSelectAll(flag)"></a-checkbox>
      <div class="content">已完成({{done}})/全部({{total}})</div>
    </div>
    <a-popconfirm title="你确定要删除吗？" ok-text="Yes" cancel-text="No" @confirm="handleDeleteAll()"> 
        <a-button type="primary" danger >清除所有已完成</a-button>
    </a-popconfirm>
  </div>
</template>
<script>
export default{
    name:"TodoFooter",
    data(){
        return{
          flag:false, 
        }
    },
    props:["list","deleteAllItem","selectAll","notSelectAll"],
    computed:{
    done() {  //计算已完成待办数量
      let num = 0;
      this.list.map(item => {
        if (item.isdone) {
          num++;
        }
      });
      return num;
    },
    total(){
        return this.list.length;
        }
    },
    methods:{
       handleDeleteAll(){
        this.deleteAllItem();
        this.flag=false;
       },
       handleSelectAll(flag){
        if(flag===false){//全选
            this.selectAll();
            this.flag=true;
        }
        if(flag===true){//全不选
            this.notSelectAll();
            this.flag=false;
        }
       }
    }
}
</script>
<style>
   .left{
  display: flex;
  align-items: center;
}
.content{
  margin-left:15px;
}
</style>
