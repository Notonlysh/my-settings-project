<template>
  <div>
    <h1>Settings</h1>

    <label>
      Username:
      <input v-model="username" />
    </label>

    <label>
      Background Color:
      <select v-model="backgroundColor">
        <option value="white">White</option>
        <option value="blue">Blue</option>
        <option value="red">Red</option>
        <option value="green">Green</option>
      </select>
    </label>

    <button @click="applySettings">Apply</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      username: '',
      backgroundColor: 'white'
    };
  },
  methods: {
    applySettings() {
      this.$emit('update-settings', {
        username: this.username,
        backgroundColor: this.backgroundColor
      });
    }
  }
};
</script>

<style scoped>
/* Add your styles here */
</style>
