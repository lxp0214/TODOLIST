<template>
  <div id="app">
      <HelloWorld msg="Welcome to Your TOTOLIST"/>
      <el-input type="text" v-model="todo" placeholder="添加TODO"></el-input>
      <el-button type="primary" v-on:click="submitEvent">提交</el-button>
      <h2>正在进行
        <span>{{todoLen}}</span>
      </h2>
      <ul >
        <li v-for="(item, index) in todoList" v-bind:key="index" v-show='item.done==false' >
          <el-button icon='el-icon-finished' v-on:click='changeStatusToHasDone(index)'></el-button>
          <span>{{item.todo}}</span>
          <el-date-picker
            v-model="timeValue"
            type="datetime"
            placeholder="选择日期时间">
          </el-date-picker>
          <el-button type="primary" icon="el-icon-delete" v-on:click='deleteTodoEvent(index,false)'></el-button>
        </li>
      </ul>
      <h2>已经完成
        <span>{{todoList.length - todoLen}}</span>
      </h2>
      <ul>
        <li v-for="(item, index) in todoList" v-bind:key="index" v-show='item.done==true'>
          <el-button icon='el-icon-finished' v-on:click='changeStatusToNotDone(index)'></el-button>
          <span>{{item.todo}}</span>
          <el-date-picker
            v-model="timeValue"
            type="datetime"
            placeholder="选择日期时间">
          </el-date-picker>
          <el-button type="primary" icon="el-icon-delete" v-on:click='deleteTodoEvent(index,true)'></el-button>
        </li>
      </ul>  
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'app',
  components: {
    HelloWorld
  },
  data () {
    return {
      todo:"",
      todoList: [],
      todoLen: 0,
      done: false,
      timeValue: '',
      pickerOptions: {
        shortcuts: [
          {
            text: '今天',
            onClick(picker) {
              picker.$emit('pick', new Date());
            }
          },
          {
            text: '昨天',
            onClick(picker) {
              const date = new Date();
              date.setTime(date.getTime() - 3600 * 1000 * 24);
              picker.$emit('pick', date);
            }
          },
          {
            text: '一周前',
            onClick(picker) {
              const date = new Date();
              date.setTime(date.getTime() - 3600 * 1000 * 24 * 7);
              picker.$emit('pick', date);
            }
          }
        ]
      },
        
    };
  },
  methods: {
    submitEvent: function () {
      let todoEvent = {
      todo: this.todo,
      done: false
      }
      this.todoList.push(todoEvent)
      this.todoLen++
      this.todo=""
    },
    deleteTodoEvent: function (index,done) {
    if(done==false) {
      this.todoLen--
    }
    this.todoList.splice(index, 1)
    },
    changeStatusToHasDone: function (index) {
      this.todoList[index].done = true
      this.todoLen--
    },
    changeStatusToNotDone: function (index) {
      this.todoList[index].done = false
      this.todoLen++
    }
  }
}
</script>

<style>
#app {
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
li {
  display: flex;
  justify-content: space-between
}

</style>
