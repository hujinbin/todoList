<template>
  <div class="hello">
    <div class="header">
 			<!-- //头部 -->
      <span class="logo">ToDoList</span>
     <!-- //keydown 键盘事件 按下enter直接添加  -->
      <input type="text"  v-model="inputstr" @keydown.enter="add"  placeholder="添加todo"
      />
    </div>
     <h1>正在进行({{nolist}})</h1>
       <!-- // 显示未完成的数量  -->
        <ul>
          <li v-for="(item,index) in list" :key="index" v-show="!item.isdone">
            <!-- //isdone 是动态添加的一个变量 用来判断todolist的完成态  -->
            <input type="checkbox" @click.prevent="handleClick(item)" />
         <!-- // 点击添加  -->
            <span class="content" @dblclick="updateIndex(item,index)" v-show="ifnum!=index">{{item.text}}</span>
            <!-- //ifnum是一个自定义变量 用来判断双击时内容的状态 -->
           <!-- //双击内容修改  -->
            <input type="text" v-show="ifnum==index" v-model="item.text" @keydown.enter="complete" @keydown.esc="cancel(item)" @blur="complete"/>

            <span @click="del(index)" class="del">x</span>
          <!-- // 删除功能 -->
          </li>
        </ul>

        <h1>已经完成({{yeslist}})</h1>
       <!-- //显示已完成数量 -->
        <ul>
          <li v-for="(item,index) in list" :key="index" v-show="item.isdone">
            <input type="checkbox" checked @click.prevent="handleyes(item)" @dblclick="updateIndex" />
            <span class="content" @dblclick="updateIndex(item,index)" v-show="ifnum!=index">{{item.text}}</span>

            <input type="text" v-show="ifnum==index" v-model="item.text" @keydown.enter="complete" @keydown.esc="cancel(item)" @blur="complete"/>

            <span @click="del(index)" class="del">x</span>
          </li>
        </ul>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      list: [],
      inputstr: "",
      ifnum: -1,
      str: ""
    };
  },
  // 计算属性
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

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
