<template>
  <div class="lifecycle-box options">
    <h2>Options API </h2>
    <p>Counter: {{ count }}</p>
    <button @click="count++">+1</button>
  </div>
</template>

<script>
export default {
  name: 'OptionsApiLifecycle',
  data() {
    return { count: 0 }
  },
  beforeCreate() {
    console.log(`----- [Options API] beforeCreate -----`);
    console.log('  -> this:', this);         // 實體已產生，但只有部分 Vue 核心屬性
    console.log('  -> this.$el:', this.$el); // null
    console.log('  -> this.$data:', this.$data); // {}
  },
  created() {
    console.log(`----- [Options API] created -----`);
    console.log('  -> this.$el:', this.$el); // null
    console.log('  -> this.$data:', this.$data); // {count: 0}
  },
  beforeMount() {
    console.log(`----- [Options API] beforeMount -----`);
    console.log('  -> this.$el:', this.$el); // null
  },
  mounted() {
    console.log(`----- [Options API] mounted -----`);
    console.log('  -> this.$el:', this.$el); // <div class="lifecycle-box options">...</div>
  },
  beforeUpdate() {
    console.log(`----- [Options API] beforeUpdate -----`);
  },
  updated() {
    console.log(`----- [Options API] updated -----`);
  },
  beforeUnmount() {
    console.log(`----- [Options API] beforeUnmount -----`);
  },
  unmounted() {
    console.log(`----- [Options API] unmounted -----`);
  }
}
</script>

<style scoped>
.lifecycle-box {
  border: 2px solid #35495e;
  border-radius: 8px;
  padding: 20px;
  margin: 10px;
  background-color: #f9f9f9;
}
h2 { color: #35495e; margin-top: 0; }
button {
  background-color: #35495e;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
}
button:hover { background-color: #2c3e50; }
</style>
