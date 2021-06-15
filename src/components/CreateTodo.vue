<template>
  <form @submit.prevent="addNewTodo">
    <div class="input-field">
      <span class="circle"></span>
      <input
        type="text"
        name="goal"
        placeholder="Currently Typing"
        v-model.trim="newTodoValue"
      />
    </div>
  </form>
  <EmptyInput :emptyInput="emptyInput" />
</template>

<script>
import { ref } from "vue";
import EmptyInput from "./EmptyInput.vue";

export default {
  name: "CreateTodo",
  components: { EmptyInput },
  emits: ["create-todo"],
  setup(_, context) {
    const emptyInput = ref(false);
    const newTodoValue = ref("");
    function addNewTodo() {
      if (newTodoValue.value !== "") {
        context.emit("create-todo", newTodoValue.value);
        newTodoValue.value = "";
      } else {
        emptyInput.value = true;
        setTimeout(() => {
          emptyInput.value = false;
        }, 2000);
      }
    }
    return { emptyInput, newTodoValue, addNewTodo };
  }
};
</script>

<style></style>
