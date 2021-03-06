<template>
  <section class="rest-of-content" :class="{ 'light-theme': lightMode }">
    <div class="todo-content">
      <TheHeader :lightMode="lightMode" @toggle-theme="toggleTheme" />
      <CreateTodo @create-todo="addNewTodo" />
      <div class="todos-container">
        <div class="main-content" v-if="goals.length > 0">
          <ul>
            <TodoItem
              v-for="(goal, index) in filteredGoals"
              :key="goal.name"
              :name="goal.name"
              :class="{ completed: goal.completed }"
              @delete="deleteGoal(index)"
              @toggle="toggleGoal(goal)"
            />
          </ul>
          <div v-if="filteredGoals.length === 0">
            <p>You don't have any goal in this section</p>
          </div>
          <div class="todos-footer">
            <span class="items-left"
              >{{ remainingGoals.length }} items left</span
            >
            <div class="filter">
              <input
                type="radio"
                v-model="selectedFilter"
                value="All"
                id="All"
              />
              <label for="All">All</label>
              <input
                type="radio"
                v-model="selectedFilter"
                value="Active"
                id="Active"
              />
              <label for="Active">Active</label>
              <input
                type="radio"
                v-model="selectedFilter"
                value="Completed"
                id="Completed"
              />
              <label for="Completed">Completed</label>
            </div>
            <div class="clear">
              <button @click="clearCompleted">clear completed</button>
            </div>
          </div>
        </div>
        <p v-else>You have not added any goals</p>
      </div>
    </div>
  </section>
</template>

<script>
import { computed, ref, watch } from "vue";
import TodoItem from "./components/TodoItem";
import TheHeader from "./components/TheHeader";
import CreateTodo from "./components/CreateTodo.vue";

export default {
  components: { TodoItem, TheHeader, CreateTodo },
  setup() {
    // Data properties used
    const emptyInput = ref(false);
    const selectedFilter = ref("All");
    const newGoalValue = ref("");
    const lightMode = ref(false);
    const goals = ref([
      {
        name: "Complete Online Javascript Course",
        completed: false
      },

      {
        name: "Jog around the pack 3x",
        completed: false
      },

      {
        name: "10 mins meditation",
        completed: false
      },

      {
        name: "Read for 1 hour",
        completed: false
      },

      {
        name: "Pick up groceries",
        completed: false
      },

      {
        name: "Complete Todo App on Frontend Mentor",
        completed: false
      }
    ]);
    // Watching Changes to Light Mode
    watch(lightMode, () => {
      localStorage.setItem("lightMode", JSON.stringify(lightMode.value));
    });
    // Fetching Data from Local Storage
    lightMode.value = JSON.parse(localStorage.getItem("lightMode"));
    goals.value = JSON.parse(localStorage.getItem("goals")) || goals.value;
    // All Functions used
    function addNewTodo(value) {
      goals.value.unshift({
        name: value,
        completed: false
      });
      updateGoalsInLocalStorage();
    }
    function deleteGoal(index) {
      goals.value.splice(index, 1);
      updateGoalsInLocalStorage();
    }
    function toggleGoal(goal) {
      goal.completed = !goal.completed;
      updateGoalsInLocalStorage();
    }
    function toggleTheme() {
      lightMode.value = !lightMode.value;
    }
    function clearCompleted() {
      goals.value = goals.value.filter(goal => !goal.completed);
      updateGoalsInLocalStorage();
    }
    function updateGoalsInLocalStorage() {
      localStorage.setItem("goals", JSON.stringify(goals.value));
    }
    // Computed Properties
    const remainingGoals = computed(() => {
      return goals.value.filter(goal => !goal.completed);
    });
    const filteredGoals = computed(() => {
      if (selectedFilter.value === "All") {
        return goals.value;
      } else if (selectedFilter.value === "Active") {
        return goals.value.filter(goal => !goal.completed);
      } else if (selectedFilter.value === "Completed") {
        return goals.value.filter(goal => goal.completed);
      }
      return goals.value;
    });
    return {
      emptyInput,
      selectedFilter,
      newGoalValue,
      lightMode,
      goals,
      addNewTodo,
      deleteGoal,
      toggleGoal,
      toggleTheme,
      clearCompleted,
      remainingGoals,
      filteredGoals
    };
  }
};
</script>

<style></style>
