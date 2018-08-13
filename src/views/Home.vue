<template>
  <div>
    <header>
      <section>
        <label for="title">ToDoList</label>
        <input type="text" v-model="todo"  @keyup.enter="addTodo" placeholder="添加ToDo" />
      </section>
    </header>
    <section>
      <h2>正在进行
        <span>{{todoLen}}</span>
      </h2>
      <ol class="demo-box">
        <li v-for="(item, index) in todoList" :key="index" v-if="item.done === false">
          <input type="checkbox" @change="changeTodo(index,true)">
          <p>{{item.todo}}</p>
          <a @click="deleteTodo(index,true)">-</a>
        </li>
      </ol>
      <h2>已经完成
        <span>{{todoList.length - todoLen}}</span>
      </h2>
      <ul>
        <li v-for="(item, index) in todoList" :key="index" v-if="item.done === true">
          <input type="checkbox" @change="changeTodo(index,false)" checked='checked'>
          <p>{{item.todo}}</p>
          <a @click="deleteTodo(index,false)">-</a>
        </li>
      </ul>
    </section>
    <footer>
      Copyright &copy; 2014 todolist.cn
      <a @click="clearData()">clear</a>
    </footer>
  </div>
</template>

<script>
import * as Utils from '@/utils/utils'
export default {
  name: 'Todolist',
  data () {
    return {
      todo: '',
      todoList: [],
      todoLen: 0
    }
  },
  methods: {
    initTodo () {
      var todoArr = Utils.getItem('todoList')
      if (todoArr) {
        for (let i = 0, len = todoArr.length; i < len; i++) {
          if (todoArr[i].done === false) {
            this.todoLen++
          }
        }
        this.todoList = todoArr
      }
    },
    addTodo () {
      let todoObj = {
        todo: this.todo,
        done: false
      }
      var tempList = Utils.getItem('todoList')
      if (tempList) {
        tempList.push(todoObj)
        Utils.setItem('todoList', tempList)
      } else {
        var tempData = []
        tempData.push(todoObj)
        Utils.setItem('todoList', tempData)
      }
      this.todoList.push(todoObj)
      this.todoLen++
      this.todo = ''
    },
    deleteTodo (index, done) {
      if(done){
        this.todoLen--
      }
      this.todoList.splice(index, 1)
      Utils.setItem('todoList', this.todoList)
      
    },
    changeTodo (index, done) {
      if (done) {
        this.todoLen--
        this.todoList[index].done = true
        Utils.setItem('todoList', this.todoList)
      } else {
        this.todoLen++
        this.todoList[index].done = false
        Utils.setItem('todoList', this.todoList)
      }
    },
    clearData () {
      localStorage.clear()
      this.todoList = []
      this.todoLen = 0
    }
  },
  mounted () {
    this.initTodo()
  }
}
</script>

<style scoped>
</style>
