<template>
  <li class="todo-item">
    <span class="main-todo">
      <span class="circle" @click="toggleGoal"></span>
      <span class="todo-name">{{ name }}</span>
    </span>
    <span class="remove-todo" @click="removeGoal"
      ><img src="../assets/images/icon-cross.svg" :alt="'Delete ' + name"
    /></span>
  </li>
</template>

<script>
export default {
  name: "TodoItem",
  props: {
    name: {
      type: String,
      required: true
    }
  },
  methods: {
    removeGoal() {
      this.$emit("delete");
    },
    toggleGoal() {
      this.$emit("toggle");
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.todos-container .main-content ul .todo-item {
  cursor: pointer;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 20px;
  border-bottom: 1px solid hsl(233, 14%, 35%);
}

.light-theme .todos-container .main-content ul .todo-item {
  border-bottom: 1px solid hsl(236, 33%, 92%);
}

.todos-container .main-content ul .todo-item .main-todo {
  display: flex;
  align-items: center;
}

.todos-container .main-content ul .todo-item .main-todo .circle {
  display: inline-block;
  height: 26px;
  width: 26px;
  border-radius: 50%;
  border: 1px solid hsl(233, 14%, 35%);
  margin-right: 20px;
  flex-shrink: 0;
  transition: all 0.5s ease;
  position: relative;
}

.light-theme .todos-container .main-content ul .todo-item .main-todo .circle {
  border: 1px solid hsl(233, 11%, 84%);
}

/* COMPLETED STATE OF TODO ITEM */
.todos-container .main-content ul .todo-item.completed .main-todo .circle {
  border: none;
}

.todos-container .main-content ul .todo-item.completed .main-todo .todo-name {
  color: hsl(237, 14%, 26%);
  text-decoration: line-through;
}

.light-theme
  .todos-container
  .main-content
  ul
  .todo-item.completed
  .main-todo
  .todo-name {
  color: hsl(233, 11%, 84%);
  text-decoration: line-through;
}

.todos-container .main-content ul .todo-item.completed .main-todo .circle {
  background: linear-gradient(
    to right,
    hsl(192, 100%, 67%) 5%,
    hsl(280, 87%, 65%) 95%
  );
}

.todos-container
  .main-content
  ul
  .todo-item.completed:hover
  .main-todo
  .circle {
  border-color: transparent;
}

/* HOVER STATE OF TODO ITEM */
.todos-container .main-content ul .todo-item:hover .main-todo .circle {
  border-color: hsl(192, 100%, 67%);
}

/* CHECK MARK WHEN TODO IS COMPLETED */
.todos-container
  .main-content
  ul
  .todo-item.completed
  .main-todo
  .circle::before {
  position: absolute;
  left: 1%;
  top: 46%;
  height: 30%;
  width: 2px;
  background-color: #fff;
  content: "";
  transform: translateX(10px) rotate(-45deg);
  transform-origin: left bottom;
}

.todos-container
  .main-content
  ul
  .todo-item.completed
  .main-todo
  .circle::after {
  position: absolute;
  left: 5%;
  bottom: 25%;
  height: 2px;
  width: 55%;
  background-color: #fff;
  content: "";
  transform: translateX(10px) rotate(-45deg);
  transform-origin: left bottom;
}
/* END CHECK MARK */

/* TODO NAME */
.todos-container .main-content ul li .todo-name {
  color: hsl(234, 39%, 85%);
  font-weight: 400;
  display: inline-block;
  padding-right: 10px;
}

.light-theme .todos-container .main-content ul li .todo-name {
  color: hsl(235, 19%, 35%);
}
/* END TODO NAME */
</style>
