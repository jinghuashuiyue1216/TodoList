<template>
  <div id="app">
    <div class="header">
      <h4 class="logo">TodoList：</h4>
      <input type="text"  v-model="inputstr" @keydown.enter="add"  placeholder="添加todo" />
    </div>
    <h4>正在进行({{nolist}}):</h4>
        <ul>
          <li v-for="(item,index) in list" :key="index" v-show="!item.isdone">
            <input type="checkbox" @click.prevent="handleClick(item)" />
            <span class="content" @dblclick="updateIndex(item,index)" v-show="ifnum!=index">{{item.text}}</span>
            <input type="text" v-show="ifnum==index" v-model="item.text" @keydown.enter="complete" @keydown.esc="cancel(item)" @blur="complete"/>
            <!-- <font color="#999">{{item.time|getParsetime}}</font> -->
            <button @click="del(index)" class="del">删除</button>
          </li>
        </ul>
    
     <h4>已经完成({{yeslist}}):</h4>
        <ul>
          <li v-for="(item,index) in list" :key="index" v-show="item.isdone">
            <input type="checkbox" checked @click.prevent="handleyes(item)" @dblclick="updateIndex" />
            <span class="content" @dblclick="updateIndex(item,index)" v-show="ifnum!=index">{{item.text}}</span>
            <input type="text" v-show="ifnum==index" v-model="item.text" @keydown.enter="complete" @keydown.esc="cancel(item)" @blur="complete"/>
            <!-- <font color="#999">{{item.time|getParsetime}}</font> -->
            <button @click="del(index)" class="del">删除</button>
          </li>
        </ul>
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
      str: ""
    };
  },
  computed: {
    yeslist() {
      let num = 0;
      this.list.map(item => {
        if (item.isdone) {
          num++;
        }
      });
      return num;
    },
    nolist() {
      let num = 0;
      this.list.map(item => {
        if (!item.isdone) {
          num++;
        }
      });
      return num;
    }
  },
  methods: {
    add() {
      this.list.push({
        text: this.inputstr,
        isdone: false,
        time: new Date().getTime()
      });
      this.inputstr = "";
      this.save();
    },
    handleClick(item) {
      item.isdone = true;
      this.save();
    },
    handleyes(item) {
      item.isdone = false;
      this.save();
    },
    updateIndex(item, index) {
      this.str = item.text;
      this.ifnum = index;
      this.save();
    },
    complete() {
      this.ifnum = -1;
      this.save();
    },
    cancel(item) {
      item.text = this.str;
      this.str = "";
      this.ifnum = -1;
      this.save();
    },

    del(index) {
      this.list.splice(index, 1);
      this.save();
    },
    save() {
      localStorage.list = JSON.stringify(this.list);
    }
  },
  created() {
    let list = localStorage.list;
    if (list) {
      this.list = JSON.parse(list);
    }
  }

   }
</script>
<style scoped>

   .header>input{
    height:20px;
   }
   ul,
  li {
     list-style: none;
   }
</style>
