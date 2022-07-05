<template>
    <div class="todo-header">
        <input type="text" placeholder="请输入你的任务名称，按回车键确认" v-model="title" @keyup.enter="add"/>
    </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'Header',
  //接受App传过来的addTodo方法
  props:{
    addTodo:{
      type:Function,
      required:true//必须要有的
    },
  },
  setup(props) {
    const title = ref('')
    //回车事件的回调函数，添加
    const add = ()=>{
      const text = title.value//ref对象，需要.value
      if(!text.trim()){
        //text.trim()是把输入的去掉空格，如果没有输入，直接返回
        alert('输入无效')
        title.value = ''//清空输入
        return
        } 
      //经过if判断，此时有数据，创建一个todo对象
      const todo ={
        id:Date.now(),
        title:text,
        isCompleted:false
      }
      //调用addTodo方法
      props.addTodo(todo)
      //清空文本框
      title.value = ''
    }
    return{
      title,
      add
    }
  }
})
</script>

<style scoped>
/*header*/
.todo-header input {
  width: 560px;
  height: 28px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 4px 7px;
}

.todo-header input:focus {
  outline: none;
  border-color: rgba(82, 168, 236, 0.8);
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075), 0 0 8px rgba(82, 168, 236, 0.6);
}
</style>