<template>
  <header class="header">
    <h3>Todo</h3>
    <transition name="swap" mode="out-in" appear>
      <button class="theme-switcher" @click="toggleTheme" v-if="!lightMode">
        <span><img src="../assets/images/icon-sun.svg" alt="Light Mode"/></span>
      </button>
      <button class="theme-switcher" @click="toggleTheme" v-else>
        <span><img src="../assets/images/icon-moon.svg" alt="Dark Mode"/></span>
      </button>
    </transition>
  </header>
</template>

<script>
export default {
  name: "TheHeader",
  props: ["lightMode"],
  emits: ["toggle-theme"],
  methods: {
    toggleTheme() {
      this.$emit("toggle-theme");
    }
  }
};
</script>

<style scoped>
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 40px;
}

.header h3 {
  color: #fff;
  font-size: 26px;
  letter-spacing: 8px;
  font-weight: 700;
  text-transform: uppercase;
}

.header .theme-switcher {
  background: none;
  outline: none;
  border: none;
  cursor: pointer;
}

.swap-enter-from,
.swap-leave-to {
  transform: translateX(-12px);
}

.swap-enter-active,
.swap-leave-active {
  transition: all 350ms ease-in-out;
}

.swap-enter-to,
.swap-leave-from {
  transform: translateX(0px);
}
</style>
