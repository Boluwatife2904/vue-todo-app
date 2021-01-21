<template>
  <section :class="{ 'light-theme': lightMode }">
    <section class="rest-of-content">
      <div class="todo-content">
        <div class="header">
          <h3>Todo</h3>
          <button class="theme-switcher" @click="toggleTheme">
            <span v-if="!lightMode"
              ><img src="../src/assets/images/icon-sun.svg" alt="Light Mode"
            /></span>
            <span v-else
              ><img src="../src/assets/images/icon-moon.svg" alt="Dark Mode"
            /></span>
          </button>
        </div>
        <form action="" @submit.prevent="addGoal">
          <div class="input-field">
            <span class="circle"></span>
            <input
              type="text"
              name="goal"
              id=""
              placeholder="Currently Typing"
              v-model="newGoalValue"
            />
          </div>
        </form>
        <div class="todos-container">
          <div class="main-content" v-if="goals.length > 0">
            <ul>
              <li
                class="todo-item"
                v-for="(goal, index) in filteredGoals"
                :key="goal.name"
                :class="{ completed: goal.completed }"
              >
                <span class="main-todo">
                  <span class="circle" @click="toggleGoal(goal)"></span>
                  <span class="todo-name">{{ goal.name }}</span>
                </span>
                <span class="remove-todo" @click="deleteGoal(index)"
                  ><img src="../src/assets/images/icon-cross.svg" alt=""
                /></span>
              </li>
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
  </section>
</template>

<script>
export default {
  data() {
    return {
      goals: [
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
      ],
      selectedFilter: "All",
      newGoalValue: "",
      lightMode: false
    };
  },
  watch: {
    lightMode() {
      localStorage.setItem("lightMode", JSON.stringify(this.lightMode));
    }
  },
  methods: {
    addGoal() {
      if (this.newGoalValue !== "") {
        this.goals.unshift({
          name: this.newGoalValue,
          completed: false
        });
        localStorage.setItem("goals", JSON.stringify(this.goals));
      }
      this.newGoalValue = "";
    },
    deleteGoal(index) {
      this.goals.splice(index, 1);
      localStorage.setItem("goals", JSON.stringify(this.goals));
    },
    toggleGoal(goal) {
      goal.completed = !goal.completed;
      localStorage.setItem("goals", JSON.stringify(this.goals));
    },
    clearCompleted() {
      this.goals = this.goals.filter(goal => goal.completed === false);
      localStorage.setItem("goals", JSON.stringify(this.goals));
    },
    toggleTheme() {
      this.lightMode = !this.lightMode;
    }
  },
  computed: {
    remainingGoals() {
      return this.goals.filter(goal => goal.completed === false);
    },
    filteredGoals() {
      if (this.selectedFilter === "All") {
        return this.goals;
      } else if (this.selectedFilter === "Active") {
        return this.goals.filter(goal => goal.completed === false);
      } else if (this.selectedFilter === "Completed") {
        return this.goals.filter(goal => goal.completed);
      }
      return this.goals;
    }
  },
  created() {
    this.lightMode = JSON.parse(localStorage.getItem("lightMode"));
    this.goals = JSON.parse(localStorage.getItem("goals")) || this.goals;
  }
};
</script>

<style>
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  margin-top: 60px;
} */
</style>
