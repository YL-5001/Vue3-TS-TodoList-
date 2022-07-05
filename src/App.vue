<template>
  <div class="todo-container">
    <div class="todo-wrap">
      <Header :addTodo='addTodo'></Header>
      <List :todos='todos' :deleteTodo='deleteTodo' :updateTodo='updateTodo'></List>
      <Footer :todos='todos' :checkAll='checkAll' :clearAllCompletedTodos='clearAllCompletedTodos'></Footer>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, toRefs } from 'vue'
//引入子组件
import Header from './components/Header.vue'
import List from './components/List.vue'
import Footer from './components/Footer.vue'
// 引入接口
import {Todo} from './types/todo'

export default defineComponent({
  name: 'App',
  components:{
    Header,
    List,
    Footer
  },
  setup() {
    //泛型接口规范todos只能是这三种数据,约束state
    const state = reactive<{ todos: Todo[]}> ({
      todos:[
        {id:1,title:'奔驰',isCompleted:false},
        {id:2,title:'宝马',isCompleted:false},
        {id:3,title:'法拉利',isCompleted:true},
      ]
    })

    //添加数据的方法
    const addTodo = (todo:Todo)=>{
      //使用unshift是在数组头部开始添加，新加的数据显示在最上面
      state.todos.unshift(todo)
    }
    //删除数据的方法
    const deleteTodo = (index:number)=>{
      state.todos.splice(index,1)
    }
    //修改todo的isCompleted属性的状态
    const updateTodo = (todo:Todo,isCompleted:boolean)=>{
      //把传入的状态赋值给todo的isCompleted
      todo.isCompleted = isCompleted
      console.log(todo)
      
    }
    //全选或全不选的方法
    const checkAll = (isCompleted:boolean)=>{
      //遍历数组
      state.todos.forEach((todo) => {
        todo.isCompleted = isCompleted
      })
    }
    //清理所有选中的数据
    const clearAllCompletedTodos = ()=> {
      state.todos = state.todos.filter(todo => !todo.isCompleted)
    }
    return{
      // 解构，就直接可以用todos了
      ...toRefs(state),
      addTodo,
      deleteTodo,
      updateTodo,
      checkAll,
      clearAllCompletedTodos
    }
  }
})
</script>

<style scoped>
/*app*/
.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>
