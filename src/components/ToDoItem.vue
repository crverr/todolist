<template>

    <li>
        <label for="">
            <input type="checkbox" :checked="item.done" @change="handleCheck(item.id)" >
            
            <!-- 效果可以实现勾选或取消，并且数据更新，但是不推荐这样做，v-model不能修改props传过来的值 -->
            <!-- <input type="checkbox" v-model="item.done" >        -->
            <span v-show="!item.isEdit">{{item.title}}</span>
            <input 
                  v-show="item.isEdit" 
                  type="text" 
                  :value="item.title" 
                  @blur="handleBlur(item, $event)"
                  ref="inputRef"
            />
        </label>

        <button class="btn btn-danger" @click="handleDel(item.id)">删除</button>
        <button v-show="!item.isEdit" class="btn btn-edit" @click="handleEdit(item)">编辑</button>
    </li>

</template>
<script>

export default ({
    name: 'ToDoItem',
    props: ['item'],
    methods: {
        //勾选与否
        handleCheck(id) {
            //通知app组件将对应todo对象的done值取反
            // this.$parent.$parent.checkTodo(id)

            //优化2：全局事件总线，发送数据
            this.$bus.$emit('checkTodo', id)
        },
        //删除
        handleDel(id) {
            if(confirm('确定删除吗？')){
              // this.$parent.$parent.delTodo(id)

              //优化2：全局事件总线，发送数据
              this.$bus.$emit('delTodo', id)
            }
        },
        //编辑
        handleEdit(todo) {
          if(todo.hasOwnProperty('isEdit')){
            todo.isEdit = true
          }else{
            //第一编辑时，没有isEdit属性

            //无效，这个不是响应式，页面不会有显示
            // todo.isEdit = true

            //需要用到$set实现响应式
            this.$set(todo, 'isEdit', true)
          }

          //推迟焦点获取方法一
          // setTimeout(() => {
          //   this.$refs.inputRef.focus()
          // }, 0);

          this.$nextTick(()=>{
            this.$refs.inputRef.focus()
          })
          
          
          
        },
        //失去焦点
        handleBlur(todo, e) {
          todo.isEdit = false
          if(!e.target.value.trim()) return alert('输入不能为空')
          this.$bus.$emit('updateTodo', todo.id, e.target.value)
        }

    }
})
</script>
<style scoped>
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
  display: none;
  margin-top: 3px;
}
li:before {
  content: initial;
}
li:last-child {
  border-bottom: none;
}
/* 鼠标在哪个数据上,哪个数据就高亮 同时显示删除按钮 */
li:hover {
  background-color: #ddd;
}
li:hover button {
  display: block;
}
li:hover {
    background-color: #ddd;
}



</style>
