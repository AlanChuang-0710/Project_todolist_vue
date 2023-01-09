<template>
  <div id="app">
    <div class="todo-container">
      <div class="todo-wrap">
        <todoHeader :todoList="todoList"></todoHeader>
        <todoMain :todoList="todoList"></todoMain>
        <todoFooter :todoList="todoList"></todoFooter>
      </div>
    </div>
  </div>
</template>

<script>
import todoHeader from "./components/todo-header.vue";
import todoFooter from "./components/todo-footer.vue";
import todoMain from "./components/todo-main.vue";

export default {
  name: "App",
  components: { todoHeader, todoFooter, todoMain },
  data() {
    return {
      todoList: JSON.parse(localStorage.getItem("todoList")) || [],
    };
  },

  watch: {
    todoList: {
      deep: true,
      handler(newVal) {
        localStorage.setItem("todoList", JSON.stringify(newVal));
      },
    },
  },

  mounted() {
    // 添加todo
    this.$bus.$on("addTodo", (todo) => {
      this.todoList.push(todo);
    });

    // 全選or全不選todo
    this.$bus.$on("checkAll", (checkAll) => {
      this.todoList.forEach((todo) => {
        todo.done = checkAll;
      });
    });

    // 清除已經完成的todo
    this.$bus.$on("clearAllDone", () => {
      if (confirm("是否清除所有已完成?")) {
        this.todoList = this.todoList.filter((todo) => !todo.done);
      }
    });

    // 勾選or不勾選checkbox
    this.$bus.$on("handleCheck", (id) => {
      this.todoList.forEach((todo) => {
        if (todo.id == id) {
          todo.done = !todo.done;
        }
      });
    });

    // 刪除todo
    this.$bus.$on("deleteTodo", (id) => {
      this.todoList = this.todoList.filter((todo) => todo.id !== id);
    });

    // 更改todo編輯狀態
    this.$bus.$on("editTodo", (id) => {
      this.todoList.forEach((todo) => {
        if (todo.id == id) {
          todo.isEdit = !todo.isEdit;
        }
      });
    });

    // 完成編輯todo
    this.$bus.$on("completeEdit", (id, newVal) => {
      this.todoList.forEach((todo) => {
        if (todo.id == id) {
          todo.isEdit = !todo.isEdit;
          if (newVal) {
            todo.mission = newVal;
          }
        }
      });
    });
  },

  beforeDestroyed() {
    this.$bus.$off("addTodo");
    this.$bus.$off("checkAll");
    this.$bus.$off("clearAllDone");
    this.$bus.$off("handleCheck");
    this.$bus.$off("deleteTodo");
    this.$bus.$off("editTodo");
    this.$bus.$off("completeEdit");
  },
};
</script>

<style lang="css">
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
  margin-right: 5px;
}

.btn-danger {
  color: #fff;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btnEdit {
  background-color: rgb(39, 88, 248);
  border: 1px solid #013bfc;
}

.btn-danger:hover {
  color: #fff;
  background-color: #bd362f;
}

.btnEdit:hover {
  background-color: rgb(1, 60, 255);
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
