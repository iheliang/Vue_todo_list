<template>
  <li>
    <label>
      <input
        type="checkbox"
        :checked="todo.done"
        @change="handleCheck(todo.id)"
      />
      <span v-show="!todo.isEdit">{{ todo.title }}</span>
      <input
        type="text"
        v-show="todo.isEdit"
        :value="todo.title"
        @blur="hangleBlur(todo, $event)"
        ref="inputTitle"
      />
    </label>
    <button class="btn btn-danger" @click="handleDelete(todo.id)">删除</button>
    <button
      v-show="!todo.isEdit"
      class="btn btn-edit"
      @click="handleEdit(todo)"
    >
      编辑
    </button>
  </li>
</template>

<script>
export default {
  name: "MyItem",
  props: ["todo"],
  methods: {
    //修改事项的勾选状态
    handleCheck(id) {
      // console.log(id);
      this.$bus.$emit("checkTodo", id);
    },
    //删除先获取到删除事件的id，再调用App里面的deleteTodo函数将事项id传入
    handleDelete(id) {
      if (confirm("确定删除吗？")) {
        this.$bus.$emit("deleteTodo", id);
      }
    },
    //编辑
    handleEdit(todo) {
      if (todo.hasOwnProperty("isEdit")) {
        todo.isEdit = true;
      } else {
        this.$set(todo, "isEdit", true);
      }
      // this.$set(todo, "isEdit", true);
      // console.log(todo);
      this.$nextTick(function () {
        this.$refs.inputTitle.focus();
      });
    },
    //失去焦点回调(真正执行修改逻辑)
    hangleBlur(todo, e) {
      todo.isEdit = false;
      if (!e.target.value.trim()) return alert("输入的内容不能为空");
      this.$bus.$emit("updateTodo", todo.id, e.target.value);
    },
  },
};
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
  display: none;
  margin-top: 3px;
}

li:before {
  content: initial;
}

li:last-child {
  border-bottom: none;
}

li:hover {
  background-color: #add;
}

li:hover button {
  display: block;
}

.todo-enter-active {
  animation: atguigu 0.5s linear;
}

.todo-leave-active {
  animation: atguigu 0.5s linear reverse;
}

@keyframes atguigu {
  from {
    transform: translateX(100%);
  }
  to {
    transform: translateX(0px);
  }
}
</style>