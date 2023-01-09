<template lang="">
  <ul class="todo-main">
    <li v-for="todo in todoList" :key="todo.id">
      <label>
        <input type="checkbox" :checked="todo.done" @change="handleCheck(todo.id)" />
        <span v-show="!todo.isEdit">{{ todo.mission }}</span>
        <input
          type="text"
          v-show="todo.isEdit"
          :value="todo.mission"
          @blur="completeEdit(todo, $event)"
          :ref="todo.id" />
      </label>
      <button class="btn btn-danger btnDelete" @click="deleteTodo(todo.id)">删除</button>
      <button class="btn btn-danger btnEdit" @click="editTodo(todo.id)">編輯</button>
    </li>
  </ul>
</template>
<script>
export default {
  name: "todo-main",
  data() {
    return {};
  },
  // 此處因為沒有把ul與li分開成兩個組件，在li上使用v-for的情況，會導致li下的$refs成為數組
  props: ["todoList"],
  methods: {
    editTodo(id) {
      this.$bus.$emit("editTodo", id);
      this.$nextTick(() => {
        // console.log(this.$refs); //單純觀察生成的東西為何
        // console.log(this.$refs[id]); //單純觀察生成的東西為何
        // console.log(this.$refs[id][0]); //單純觀察生成的東西為何
        this.$refs[id][0].focus();
      });
    },

    deleteTodo(id) {
      if (confirm(`確定刪除?`)) {
        this.$bus.$emit("deleteTodo", id);
      }
    },

    handleCheck(id) {
      this.$bus.$emit("handleCheck", id);
    },

    completeEdit(todo, event) {
      if (!event.target.value) {
        todo.isEdit = false;
        event.target.value = todo.mission;
        return alert("輸入值不能為空!");
      }
      this.$bus.$emit("completeEdit", todo.id, event.target.value);
    },
  },
};
</script>

<style lang="css" scoped>
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

li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

li:hover {
  background-color: #ddd;
}

li:hover button {
  display: block;
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
</style>
