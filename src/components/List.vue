<template>
    <ul class="todo-main">
        <!-- :todo="todo"是把从App接收到的todo再传给Item子组件 -->
        <Item 
            v-for="(todo,index) in todos" 
            :key="todo.id" 
            :todo="todo" 
            :deleteTodo="deleteTodo"
            :updateTodo="updateTodo"
            :index="index"
        >
        </Item>
    </ul>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
//引入子组件
import Item from './Item.vue'

export default defineComponent({
  name: 'List',
  components:{
      Item
  },
  //接收App组件传过来的todos
  props:['todos','deleteTodo','updateTodo']
})
</script>

<style scoped>
/*main*/
.todo-main {
  margin-left: 0px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0px;
}

.todo-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
}
</style>