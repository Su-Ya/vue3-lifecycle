<template>
  <!-- 用 ref="rootEl" 綁定來取得 DOM 元素 -->
  <div class="lifecycle-box composition" ref="rootEl">
    <h2>Composition API</h2>
    <p>Counter: {{ count }}</p>
    <button @click="count++">+1</button>
  </div>
</template>

<script setup>
import { ref, onBeforeMount, onMounted, onBeforeUpdate, onUpdated, onBeforeUnmount, onUnmounted, getCurrentInstance } from 'vue';

defineOptions({
  name: 'CompositionApiLifecycle'
});

const props = defineProps({
  msg: {
    type: String,
    default: ''
  }
});

const count = ref(0);

// 代替 this.$el 取 DOM 的方式
const rootEl = ref(null);

// 如果真的需要取得當前 Vue instance (通常不建議，保留給進階 plugin 開發)
const instance = getCurrentInstance();

const printStatus = (hook) => {
  console.log(`----- [Composition API] ${hook} -----`);
  console.log('  -> 在 setup 裡沒有 this:', this);// 在 <script setup> 中，this 會是 undefined。
  console.log('  -> Vue Instance 透過 getCurrentInstance():', instance);
  console.log('  -> props.msg:', props.msg);
  console.log('  -> DOM Element, rootEl.value:', rootEl.value);
  console.log('  -> Reactivity ref(), count.value:', count.value);
};

// setup()
printStatus('setup');

onBeforeMount(() => {
  printStatus('onBeforeMount');
});

onMounted(() => {
  printStatus('onMounted');
});

onBeforeUpdate(() => {
  console.log(`----- [Composition API] onBeforeUpdate -----`);
});

onUpdated(() => {
  console.log(`----- [Composition API] onUpdated -----`);
});

onBeforeUnmount(() => {
  console.log(`----- [Composition API] onBeforeUnmount -----`);
});

onUnmounted(() => {
  console.log(`----- [Composition API] onUnmounted -----`);
});
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
