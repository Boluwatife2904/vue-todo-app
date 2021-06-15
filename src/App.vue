<template>
  <section class="rest-of-content" :class="{ 'light-theme': lightMode }">
    <div class="todo-content">
      <TheHeader :lightMode="lightMode" @toggle-theme="toggleTheme" />
      <form @submit.prevent="addNewGoal">
        <div class="input-field">
          <span class="circle"></span>
          <input
            type="text"
            name="goal"
            placeholder="Currently Typing"
            v-model.trim="newGoalValue"
          />
        </div>
      </form>
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
    <EmptyInput :emptyInput="emptyInput" />
  </section>
</template>

<script>
import { ref, watch } from "vue";
import TodoItem from "./components/TodoItem";
import TheHeader from "./components/TheHeader";
import EmptyInput from "./components/EmptyInput";

export default {
  components: { TodoItem, TheHeader, EmptyInput },
  computed: {
    remainingGoals() {
      return this.goals.filter((goal) => !goal.completed);
    },
    filteredGoals() {
      if (this.selectedFilter === "All") {
        return this.goals;
      } else if (this.selectedFilter === "Active") {
        return this.goals.filter((goal) => !goal.completed);
      } else if (this.selectedFilter === "Completed") {
        return this.goals.filter((goal) => goal.completed);
      }
      return this.goals;
    },
  },
  setup() {
    // Data properties used
    const emptyInput = ref(false);
    const selectedFilter = ref("All");
    const newGoalValue = ref("");
    const lightMode = ref(false);
    const goals = ref([
      {
        name: "Complete Online Javascript Course",
        completed: false,
      },

      {
        name: "Jog around the pack 3x",
        completed: false,
      },

      {
        name: "10 mins meditation",
        completed: false,
      },

      {
        name: "Read for 1 hour",
        completed: false,
      },

      {
        name: "Pick up groceries",
        completed: false,
      },

      {
        name: "Complete Todo App on Frontend Mentor",
        completed: false,
      },
    ]);
    // Watching Changes to Light Mode
    watch(lightMode, (_, newValue) => {
      localStorage.setItem("lightMode", JSON.stringify(newValue));
    });
    // Fetching Data from Local Storage
    lightMode.value = JSON.parse(localStorage.getItem("lightMode"));
    goals.value = JSON.parse(localStorage.getItem("goals")) || goals.value;
    // All Functions used
    function addNewGoal() {
      if (newGoalValue.value !== "") {
        goals.value.unshift({
          name: newGoalValue.value,
          completed: false
        });
        updateGoalsInLocalStorage();
      } else {
        emptyInput.value = true;
        setTimeout(() => {
          emptyInput.value = false;
        }, 2000);
      }
      this.newGoalValue = "";
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
    return {
      emptyInput,
      selectedFilter,
      newGoalValue,
      lightMode,
      goals,
      addNewGoal,
      deleteGoal,
      toggleGoal,
      toggleTheme,
      clearCompleted
    };
  }
};
</script>

<style></style>
