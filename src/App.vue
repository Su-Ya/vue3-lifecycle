<template>
  <div id="app-container">
    <header>
      <h1>Vue 3 Lifecycle Demo</h1>
      <p>Toggle components or click the counter to observe lifecycles in the Console Viewer.</p>
    </header>

    <main class="main-layout">
      <div class="lifecycle-image-container">
        <img src="https://vuejs.org/assets/lifecycle.MuZLBFAS.png" alt="Vue 3 Lifecycle Diagram" />
      </div>
      
      <div>
        <div class="controls">
          <label>
            <input type="checkbox" v-model="showComposition" />
            Composition API
          </label>
          <label>
            <input type="checkbox" v-model="showOptions" />
            Options API
          </label>
        </div>
        <div class="components-container">
          <CompositionApiLifecycle v-if="showComposition" />
          <OptionsApiLifecycle v-if="showOptions" />
        </div>
        <!-- 全域 Console 攔截顯示區塊 -->
        <div class="global-console">
          <div class="console-header">
            <h3>Console Viewer</h3>
            <button class="clear-btn" @click="consoleLogs = []">Clear</button>
          </div>
          <div class="log-lines">
            <div v-for="(log, i) in consoleLogs" :key="i" class="log-line">
              <span class="log-time">[{{ log.time }}]</span>
              <span class="log-msg" :class="{ 'highlight-msg': log.isHighlight }">{{ log.msg }}</span>
            </div>
            <div v-if="consoleLogs.length === 0" style="color:#666; font-style:italic">No logs yet...</div>
          </div>
        </div>
      </div>
      
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import CompositionApiLifecycle from './components/CompositionApiLifecycle.vue';
import OptionsApiLifecycle from './components/OptionsApiLifecycle.vue';

const showComposition = ref(true);
const showOptions = ref(true);

// === 全域 Console 攔截器 (攔截 console.log 並轉繪至畫面) ===
const consoleLogs = ref([]);
const originalLog = console.log;

// 建立一個全域序號來追蹤
let globalLogIndex = 0;

console.log = (...args) => {
  // 原本的照樣輸出到開發者工具
  originalLog.apply(console, args);
  
  globalLogIndex++;
  const d = new Date();
  const timeStr = `${d.getHours().toString().padStart(2,'0')}:${d.getMinutes().toString().padStart(2,'0')}:${d.getSeconds().toString().padStart(2,'0')}.${d.getMilliseconds().toString().padStart(3,'0')}`;

  const msgStr = args.map(arg => {
    if (arg === undefined) return 'undefined';
    if (arg === null) return 'null';
    if (typeof arg === 'object') {
      try {
        // 簡易標記 option api Vue Instance
        if (Object.hasOwn(arg, '$el')) return `[Vue Component Instance]`;
        
        // 簡易標記 composition api Vue Instance
        if (Object.hasOwn(arg, 'uid')) return `[Vue Component Instance]`;
        
        if (arg instanceof HTMLElement) {
          const attrs = Array.from(arg.attributes)
            .map(attr => attr.value === '' ? attr.name : `${attr.name}="${attr.value}"`)
            .join(' ');
          const tag = arg.tagName.toLowerCase();
          return attrs ? `<${tag} ${attrs}>` : `<${tag}>`;
        }
        
        return JSON.stringify(arg);
      } catch (e) {
        return Object.prototype.toString.call(arg); // fallback
      }
    }
    return String(arg);
  }).join(' ');

  const isHighlight = msgStr.includes('-----') && msgStr.includes('API');
  consoleLogs.value.push({ time: `${globalLogIndex} | ${timeStr}`, msg: msgStr, isHighlight });
};
</script>

<style>
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 20px;
  background-color: #f0f2f5;
  color: #333;
}
#app-container {
  background: white;
  padding: 30px;
  border-radius: 12px;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
}
header {
  text-align: center;
  margin-bottom: 30px;
}
.controls {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-bottom: 20px;
  padding: 15px;
  background: #e9ecef;
  border-radius: 8px;
}
.controls label {
  font-size: 1.1em;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 8px;
}
.main-layout {
  display: grid;
  grid-template-columns: 1.2fr 1fr;
  grid-template-areas:
    "components image"
    "console image";
  gap: 20px;
  align-items: start;
}
@media (max-width: 900px) {
  .main-layout {
    grid-template-columns: 1fr;
    grid-template-areas:
    "components"
    "console"
    "image";
  }
}
.lifecycle-image-container {
  grid-area: image;
  display: flex;
  justify-content: center;
  position: sticky;
  top: 20px;
  margin-top: 65px;
}
@media (max-width: 900px) {
  .lifecycle-image-container {
    margin-top: 0px;
  }
}
.lifecycle-image-container img {
  max-width: 100%;
  height: auto;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
}
.components-container {
  grid-area: components;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}
@media (max-width: 768px) {
  .components-container {
    grid-template-columns: 1fr;
  }
}

/* Console 樣式 */
.global-console {
  grid-area: console;
  border: 1px solid #333;
  border-radius: 6px;
  background: #1e1e1e;
  overflow: hidden;
}
.console-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #2d2d2d;
  padding: 10px 15px;
  border-bottom: 1px solid #444;
}
.console-header h3 {
  color: #ccc;
  margin: 0;
  font-size: 1rem;
}
.clear-btn {
  background: #33a06f;
  color: white;
  border: none;
  padding: 4px 12px;
  border-radius: 4px;
  cursor: pointer;
}
.clear-btn:hover { background: #42b883; }
.log-lines {
  height: 400px;
  overflow-y: auto;
  padding: 15px;
  font-family: 'Consolas', 'Courier New', monospace;
  font-size: 14px;
}
.log-line {
  margin-bottom: 4px;
  padding-bottom: 4px;
  border-bottom: 1px dashed #333;
  display: flex;
  gap: 12px;
}
.log-line:last-child {
  border-bottom: none;
}
.log-time {
  color: #888;
  white-space: nowrap;
}
.log-msg {
  color: #c5c8c6;
  word-break: break-all;
}
.log-msg.highlight-msg {
  color: #ff595d;
}
</style>
