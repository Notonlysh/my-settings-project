<template>
  <div class="component-container">
    <img src="@/assets/logo.png" alt="Logo" class="component-logo" />
    <h1>AppCalender</h1>
  </div>
</template>

<script>
export default {
  name: "AppCalender.vue",
  data() {
    return {
      activeComponent: null,  // This will store the name of the active component
      // ... your existing data ...
      leftComponents: [
        // ... 你的代码
      ],
      rightComponents: [
        // ... 你的代码
      ]
    }
  }

}
</script>

<style scoped>
.component-container {
  display: flex;
  align-items: center; /* This vertically centers the icon with the text */
}
.component-logo {
  /* Add any styling you want for your logo here */
  width: 30px;
  height: 30px;
  margin-right: 10px;
}
</style>