<template>
  <div class="todo-footer" v-show="total">
    <label>
      <!-- <input type="checkbox" :checked="isAll" @change="checkAll" /> -->
      <input type="checkbox" v-model="isAll" />
    </label>
    <span>
      <span>已完成{{ doneTotal }}</span> / 全部{{ total }}
    </span>
    <button class="btn btn-danger" @click="clearAll">清除已完成任务</button>
  </div>
</template>

<script>
export default {
  name: "MyFooter",
  props: ["todos"], //["checkAllTodo", "clearAllTodo"]
  computed: {
    //统计事项的总数
    total() {
      return this.todos.length;
    },
    //统计已完成的事项总数
    doneTotal() {
      return this.todos.reduce((pre, todo) => pre + (todo.done ? 1 : 0), 0);
    },
    //底部勾选双向数据绑定
    isAll: {
      //默认先读取一遍数据是否全部勾选，全勾选的话会把底部的勾选框自动勾上
      get() {
        return this.total === this.doneTotal && this.total > 0;
      },
      //如检测到改变了底部勾选框则调用App的checkAllTodo函数
      set(value) {
        //this.ckeckAllTodo(value)
        this.$emit("checkAllTodo", value);
      },
    },
  },
  methods: {
    //底部删除按钮被点击则调用该函数
    clearAll() {
      // this.clearAllTodo();
      this.$emit("clearAllTodo");
    },
  },
};
</script>

<style scoped>
/*footer*/
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