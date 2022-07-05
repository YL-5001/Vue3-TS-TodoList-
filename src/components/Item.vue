<template>
  <li @mouseenter="mouseHandler(true)" @mouseleave="mouseHandler(false)"
    :style="{ backgroundColor: bgColor, color: myColor }">
    <label>
      <!-- 父级组件的数据最好不要让子集组件直接修改，需要修改就让子集组件去调用父级组件的方法，
        调用才能修改，所以用了计算属性isComplete -->
      <input type="checkbox" v-model="isComplete" />
      <span>{{todo.title}}</span>
    </label>
    <button class="btn btn-danger" v-show="isShow" @click="delTodo">删除</button>
  </li>
</template>

<script lang="ts">
  import {
    computed,
    defineComponent,
    ref
  } from 'vue'
  //引入接口
  import {
    Todo
  } from '../types/todo'

  export default defineComponent({
    name: 'Item',
    //接收List传入过来的todo
    props: {
      todo: {
        type: Object as() => Todo, //函数返回是Todo类型，这里还需要调用接口
        required: true
      },
      deleteTodo: {
        type: Function,
        required: true
      },
      index: {
        type: Number,
        required: true
      },
      updateTodo: {
        type: Function,
        required: true
      }
    },
    setup(props) {
      //背景色
      const bgColor = ref('white')
      const myColor = ref('black')
      //是否显示删除按钮
      const isShow = ref(false)
      //鼠标进入和离开事件的回调函数
      const mouseHandler = (flag: boolean) => {
        if (flag) {
          //鼠标进入
          bgColor.value = 'skyblue'
          myColor.value = 'white'
          isShow.value = true
        } else {
          //鼠标离开
          bgColor.value = 'white'
          myColor.value = 'black'
          isShow.value = false
        }
      }
      //删除数据的方法
      const delTodo = () => {
        if (window.confirm('确定要删除吗？')) {
          props.deleteTodo(props.index)
        }
      }
      //计算属性的方式------让当前复选框选中/不选中
      const isComplete = computed({
        get() {
          return props.todo.isCompleted
        },
        set(val) {
          props.updateTodo(props.todo, val)
        }
      })
      return {
        mouseHandler,
        bgColor,
        myColor,
        isShow,
        delTodo,
        isComplete
      }
    }
  })
</script>

<style scoped>
  /*item*/
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
    /* display: none; */
    margin-top: 3px;
  }

  li:before {
    content: initial;
  }

  li:last-child {
    border-bottom: none;
  }
</style>