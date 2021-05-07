<template>
  <teleport to="#app">
    <div class="empty" :class="{ show: emptyInput }">
      <img src="../assets/images/alert-triangle.svg" alt="Alert Triangle" />
      You have not entered any value
    </div>
  </teleport>
</template>

<script>
export default {
  name: "EmptyImput",
  props: ["emptyInput"]
};
</script>

<style scoped>
.empty {
  display: flex;
  align-items: center;
  border-radius: 4px;
  background: rgb(255, 145, 0);
  position: absolute;
  top: -100%;
  right: 16px;
  color: #fff;
  font-size: 16px;
  font-weight: 600;
  padding: 20px 30px;
  z-index: 999;
  transition: all 0.5s ease-in-out;
}

.empty.show {
  top: 60px;
}

.empty img {
  margin-right: 10px;
  height: 20px;
  width: 20px;
  display: block;
}
</style>
