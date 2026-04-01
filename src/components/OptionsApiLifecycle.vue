<template>
  <div class="lifecycle-box options" ref="rootEl">
    <h2>Options API </h2>
    <p>Counter: {{ count }}</p>
    <button @click="count++">+1</button>
  </div>
</template>

<script>
export default {
  name: 'OptionsApiLifecycle',
  props: {
    msg: {
      type: String,
      default: ''
    }
  },
  data() {
    return { count: 0 }
  },
  methods: {
    printStatus(hook) {
      console.log(`----- [Options API] ${hook} -----`);
      console.log('  -> this:', this);
      console.log('  -> props.msg:', this.msg);
      console.log('  -> DOM, this.$refs.rootEl:', this.$refs.rootEl);
      console.log('  -> Reactivity, this.$data:', this.$data);
    }
  },
  beforeCreate() {
    console.log(`----- [Options API] beforeCreate -----`);
    console.log('  -> this:', this);         // 實體已產生，但只有部分 Vue 核心屬性
    console.log('  -> props.msg:', this.msg);
    // console.log('  -> DOM Element, this.$el:', this.$el); // null
    console.log('  -> DOM, this.$refs.rootEl:', this.$refs.rootEl); // undefined
    console.log('  -> Reactivity, this.$data:', this.$data); // {}
  },
  created() {
    this.printStatus('created');
    // console.log('  -> DOM Element, this.$el:', this.$el); // null
    // console.log('  -> DOM, this.$refs.rootEl:', this.$refs.rootEl); // undefined
    // console.log('  -> Reactivity, this.$data:', this.$data); // {count: 0}
  },
  beforeMount() {
    this.printStatus('beforeMount');
    // console.log('  -> DOM Element, this.$el:', this.$el); // null
    // console.log('  -> DOM, this.$refs.rootEl:', this.$refs.rootEl); // undefined
  },
  mounted() {
    this.printStatus('mounted');
    // console.log('  -> DOM Element, this.$el:', this.$el); // <div class="lifecycle-box options">...</div>
    // console.log('  -> DOM, this.$refs.rootEl:', this.$refs.rootEl); // <div class="lifecycle-box options">...</div>
  },
  beforeUpdate() {
    this.printStatus('beforeUpdate');
  },
  updated() {
    this.printStatus('updated');
  },
  beforeUnmount() {
    this.printStatus('beforeUnmount');
  },
  unmounted() {
    this.printStatus('unmounted');
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
