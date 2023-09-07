<template>
    <div class="todo-footer" v-show="total">
        <label for="">
            <!-- 方法1 效果也可实现 -->
            <!-- <input type="checkbox" :checked="isAll" @change="checkAll"> -->

            <!-- 方法2 效果一样，更精简 -->
            <input type="checkbox" v-model="isAll">
        </label>
        <span><span>已完成{{doneTotal}}</span> / 全部{{total}}</span>
        <button class="btn btn-danger" @click="clearAll">删除已完成任务</button>
    </div>
</template>
<script>


export default ({
    name: 'MyFooter',
    props: ['todos'],
    computed: {
        total() {
            return this.todos.length
        },
        doneTotal() {
            return this.todos.reduce((pre,todo)=>{
                return pre + (todo.done ? 1: 0)
            }, 0)
        },
        //方法1
        // isAll() {
        //     return this.total === this.doneTotal && this.total >0
        // },
        //方法2
        isAll: {
            get() {
                return this.total === this.doneTotal && this.total >0
            },
            set(value) {
                this.$parent.checkAllTodo(value)
                //优化1：也可以用用自定义事件￥emit
                // this.$emit('checkAllTodo', value)

            }
        }
        
    },
    methods: {
        //方法1
        // checkAll(e) {
        //     // console.log(e.target.checked);
        //     this.$parent.checkAllTodo(e.target.checked)
        // }
        clearAll() {
            if(confirm('确定删除吗？'))
                this.$parent.clearAllTodo()
                //优化1：也可以用用自定义事件￥emit
                // this.$emit('clearAllTodo')
        }
    }
})
</script>

<style scoped>
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}
.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}
.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}
.todo-footer button {
  float: right;
  margin-top: 5px;
}

</style>
