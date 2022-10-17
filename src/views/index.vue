<template>
  <div id="app">
    <div class="container">
     <div class="container-wrap">
      <div class="header">
        <h4 class="logo">今日待办：</h4>
        <a-input type="text"  v-model="inputstr" @keydown.enter="add" placeholder="输入代办事项,按回车添加" />
      </div>
      <a-list size="small">
          <a-list-item class="item" v-for="(item,index) in list" :key="index" >          
           <div class="left">
            <a-checkbox :checked="item.isdone" @click="handleClick(item)"></a-checkbox>
            <div class="content">
              <div @dblclick="updateIndex(item,index)" v-if="ifnum!=index">{{item.text}}</div>
              <a-input type="text" v-if="ifnum==index" v-model="item.text" @blur="complete"/> 
            </div>            
           </div>
           <a-popconfirm title="你确定要删除吗？" ok-text="Yes" cancel-text="No" @confirm="confirm(index)"> 
            <a-button type="primary" danger >删除</a-button>
           </a-popconfirm>
          </a-list-item>
      </a-list>
      <div class="footer">
        <h4>已完成({{yeslist}})/全部({{list.length}})</h4>
        <!-- <a-popconfirm title="你确定要删除吗？" ok-text="Yes" cancel-text="No" @confirm="deleteAll"> 
            <a-button type="primary" danger >清除所有已完成</a-button>
        </a-popconfirm> -->
      </div>
     </div>
    </div>
  </div>
</template>
<script>
   import 'ant-design-vue/dist/antd';
   import { message } from 'ant-design-vue';
import { del } from 'vue';
   export default{
    name: "app",
    data() {
    return {
      list: [],
      inputstr: "",
      ifnum: -1,
     // str: ""
    };
  },
  computed: {
    yeslist() {  //计算已完成待办数量
      let num = 0;
      this.list.map(item => {
        if (item.isdone) {
          num++;
        }
      });
      return num;
    }
  },
  methods: {
    add() {
      this.list.unshift({  //添加一个待办对象到数组
        text: this.inputstr,
        isdone: false
      });
      this.inputstr = "";//不要忘了将该变量置为空
      this.save();
    },
    handleClick(item) {
      item.isdone=!item.isdone;
      this.save();
      console.log(this.list)
    },
    updateIndex(item, index) {//双击以后隐藏span,显示input
     // this.str = item.text;
      this.ifnum = index;
      this.save();
    },
    complete() {//修改完成后点击空白处，隐藏input,显示span
      this.ifnum = -1;
      this.save();
    },
    save() {//执行添加、删除操作后也要用localstorage来存储到本地
      localStorage.list = JSON.stringify(this.list);
    },
    confirm(index){
      this.list.splice(index, 1);
      console.log(this.list);
      this.save();
      message.success('已删除');
    },
    // deleteAll(e){
    //   console.log(e);
    //   this.save();
    //   message.success('已删除');
    // }
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
   
