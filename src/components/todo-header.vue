<template lang="">
  <div class="todo-header">
    <input type="text" @keyup.enter="addTodo" v-model="title" placeholder="請輸入你的任務名稱後，Enter確認" />
  </div>
</template>
<script>
import { nanoid } from "nanoid";
export default {
  name: "todo-header",
  props: ["todoList"],
  data() {
    return {
      title: "",
    };
  },
  methods: {
    addTodo() {
      if (!this.title) return alert("輸入值不能為空");
      const todo = { mission: this.title, id: nanoid(), done: false, isEdit: false };
      this.$bus.$emit("addTodo", todo);
      this.title = "";
    },
  },
};
</script>
<style lang="css" scoped>
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
