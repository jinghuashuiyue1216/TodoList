<template>
  <div id="app">
    <div class="container">
     <div class="container-wrap">
      <div class="header">
        <h4 class="logo">今日待办：</h4>
        <input type="text"  v-model="inputstr" @keydown.enter="add" placeholder="输入代办事项,按回车添加" />
      </div>
      <h4>未完成({{nolist}}):</h4>
      <ul>
          <li v-for="(item,index) in list" :key="index" v-if="!item.isdone" >          
              <input type="checkbox" @click.prevent="handleClick(item)" />
              <span class="content" @dblclick="updateIndex(item,index)" v-if="ifnum!=index">{{item.text}}</span>
              <input type="text" v-if="ifnum==index" v-model="item.text" @blur="complete"/>           
            <button @click="del(index)" class="btn btn-danger">删除</button>
          </li>
      </ul>
      <h4>已完成({{yeslist}}):</h4>
      <ul>
          <li v-for="(item,index) in list" :key="index" v-if="item.isdone" >         
              <input type="checkbox" checked @click.prevent="handleyes(item)" @dblclick="updateIndex" />
              <span class="content" @dblclick="updateIndex(item,index)" v-if="ifnum!=index">{{item.text}}</span>
              <input type="text" v-show="ifnum==index" v-model="item.text" @blur="complete"/>
            <button @click="del(index)" class="btn btn-danger">删除</button>
          </li>
      </ul>
     </div>
    </div>
  </div>
</template>
<script>
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
    },
    nolist() {  //计算未完成待办数量
      let num = 0;
      this.list.map(item => {  //遍历list里的数组成员
        if (!item.isdone) {
          num++;
        }
      });
      return num;
    }
  },
  methods: {
    add() {
      this.list.push({  //添加一个待办对象到数组
        text: this.inputstr,
        isdone: false,
        //time: new Date().getTime()
      });
      this.inputstr = "";//不要忘了将该变量置为空
      this.save();
    },
    handleClick(item) {//checkbox表单自身有默认行为，点击后会默认选中，会影响下面的判断，要用prevent来阻止
      item.isdone = true;
      this.save();
    },
    handleyes(item) {
      item.isdone = false;
      this.save();
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
    // cancel(item) {
    //   //item.text = this.str;
    //   this.str = "";
    //   this.ifnum = -1;
    //   this.save();
    // },

    del(index) {
      this.list.splice(index, 1);
      this.save();
    },
    save() {//执行添加、删除操作后也要用localstorage来存储到本地
      localStorage.list = JSON.stringify(this.list);
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
.header input{
  width: 560px;
  height: 28px;
  font-size: 14px;
}


input{
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 4px 7px;
}
 input:focus {
  outline: none;
  border-color: rgba(82, 168, 236, 0.8);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075),
    0 0 8px rgba(82, 168, 236, 0.6);
}
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}
.btn {
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
}

li {
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
}

li button {
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
}

</style>
   
