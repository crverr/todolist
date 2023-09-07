<template>
  <div id="root">
    <div class="todo-container">
      <div class="todo-wrap">
        <my-header @addTodo="addTodo"></my-header>
        <to-do-list :todos="todos" ></to-do-list>
        <my-footer :todos="todos"></my-footer>
        <!-- 优化1：也可以用自定义事件，v-on  -->
        <!-- <my-footer :todos="todos" @checkAllTodo="checkAllTodo" @clearAllTodo="clearAllTodo"></my-footer> -->
      </div>
    </div>
  </div>
</template>
<script>
  import MyHeader from '@/components/MyHeader'
  import MyFooter from '@/components/MyFooter'
  import ToDoList from '@/components/ToDoList'

  export default ({
    name: 'App',
    components: {
      MyHeader,
      MyFooter,
      ToDoList
    },
    data() {
        return {
          //从浏览器缓存中读取数据
          todos: JSON.parse(localStorage.getItem('todos')) || []
        }
        
    },
    methods: {
      addTodo(todoObj){
        this.todos.unshift(todoObj)

      },
      //取消or勾选一个todo
      checkTodo(id) {
        this.todos.forEach(todo => {
          if(todo.id === id) todo.done = !todo.done
        });
      },
      //删除一个todo
      delTodo(id) {
        this.todos = this.todos.filter(todo => todo.id !== id)
      },
      //全选
      checkAllTodo(done) {
        this.todos.forEach(todo => {
          todo.done = done
        })
      },
      //删除已完成的任务
      clearAllTodo() {
        this.todos = this.todos.filter(todo=>{
          return !todo.done
        })
      },
      //更新
      updateTodo(id,title) {
        this.todos.forEach(todo => {
          if(todo.id === id) todo.title = title
        })
      }
    },
    watch: {
      //监测todos数据变化
      todos: {
        //深度监视，监视对象里的属性变化
        deep: true,
        handler(value) {
          // console.log(value);
          localStorage.setItem('todos',JSON.stringify(value))
        }
      }
    },
    mounted() {
      //优化2：全局事件总线，APP接收数据，所以绑定自定义事件
      this.$bus.$on("checkTodo", this.checkTodo)
      this.$bus.$on('delTodo', this.delTodo)
      this.$bus.$on('updateTodo', this.updateTodo)
    },
    //使用完之后 beforeDestroy解绑自定义事件
    beforeDestroy() {
      this.$bus.$off('checkTodo')
      this.$bus.$off('delTodo')
    }
    
  })
</script>

<style lang="less">
body {
  background: #fff;
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
  box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.2), 0 1px 2px rgba(0, 0, 0, 0.05);
  border-radius: 4px;
}
.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}
.btn-edit {
  color: #fff;
  background-color: #3ca9ed;
  border: 1px solid #0a7cb9;
  margin-right: 10px;
}
.btn-edit:hover {
  color: #fff;
  background-color: #0a7cb9;
}
.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}
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
