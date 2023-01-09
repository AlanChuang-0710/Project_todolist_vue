<template>
  <div class="todo-footer">
    <label>
      <input type="checkbox" v-model="isAll" />
    </label>
    <span>
      <span>已完成 {{ doneNum }}</span> / 全部 {{ total }}
    </span>
    <button class="btn btn-danger" @click="clearAllDone">清除已完成任务</button>
  </div>
</template>
<script>
export default {
  name: "todo-footer",
  data() {
    return {};
  },

  props: ["todoList"],

  computed: {
    doneNum() {
      return this.todoList.filter((todo) => todo.done == true).length;
    },
    total() {
      return this.todoList.length;
    },
    isAll: {
      get() {
        return this.doneNum === this.total && this.total > 0;
      },
      set(value) {
        this.$bus.$emit("checkAll", value);
      },
    },
  },

  methods: {
    clearAllDone() {
      this.$bus.$emit("clearAllDone");
    },
  },
};
</script>

<style lang="css" scoped>
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
