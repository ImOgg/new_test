<script setup lang="ts">
import { useMouse, useStorage, useColorMode } from '@vueuse/core';

// 從 JSON Server 獲取便箋
const { data: notes, isPending, isError, error } = useQuery({
  queryKey: ['notes'],
  queryFn: () => $fetch('http://localhost:3001/notes')
});

// 使用 VueUse 的 useStorage 來保存筆記拖曳後的位置
const savedPositions = useStorage('note-positions', {});

// 使用 VueUse 的 useMouse 獲取滑鼠位置
const { x: mouseX, y: mouseY } = useMouse();

// 使用 VueUse 的 useColorMode 實現暗黑模式切換
const colorMode = useColorMode();

// 拖曳狀態管理
const draggingNoteId = ref(null);
const dragOffset = ref({ x: 0, y: 0 });

// 開始拖曳
function startDrag(noteId, event) {
  draggingNoteId.value = noteId;
  const note = notes.value.find(n => n.id === noteId);
  const notePosition = savedPositions.value[noteId] || { x: note.x, y: note.y };
  
  dragOffset.value = {
    x: event.clientX - notePosition.x,
    y: event.clientY - notePosition.y
  };
}

// 拖曳中
function onDrag() {
  if (draggingNoteId.value !== null) {
    savedPositions.value[draggingNoteId.value] = {
      x: mouseX.value - dragOffset.value.x,
      y: mouseY.value - dragOffset.value.y
    };
  }
}

// 結束拖曳
function stopDrag() {
  draggingNoteId.value = null;
}

// 取得便箋位置
function getNotePosition(note) {
  return savedPositions.value[note.id] || { x: note.x, y: note.y };
}

// 添加新便箋
const newNoteContent = ref('');

function addNewNote() {
  if (newNoteContent.value.trim()) {
    // 在真實應用中，這裡應該發送 POST 請求到 JSON Server
    alert(`新增便箋: ${newNoteContent.value}`);
    newNoteContent.value = '';
  }
}

// 監聽拖曳事件
onMounted(() => {
  window.addEventListener('mousemove', onDrag);
  window.addEventListener('mouseup', stopDrag);
});

onUnmounted(() => {
  window.removeEventListener('mousemove', onDrag);
  window.removeEventListener('mouseup', stopDrag);
});
</script>

<template>
  <div class="notes-app" :class="{ 'dark-mode': colorMode.value === 'dark' }">
    <div class="header">
      <h1 class="text-xl font-bold">便箋牆</h1>
      
      <div class="actions">
        <button @click="colorMode.value = colorMode.value === 'dark' ? 'light' : 'dark'" class="mode-toggle">
          {{ colorMode.value === 'dark' ? '🌞' : '🌙' }}
        </button>
        
        <div class="add-note-form">
          <input 
            v-model="newNoteContent" 
            placeholder="新增便箋..."
            @keyup.enter="addNewNote"
          />
          <button @click="addNewNote">新增</button>
        </div>
      </div>
    </div>

    <div class="notes-container">
      <p v-if="isPending">載入中...</p>
      <p v-else-if="isError" class="error">錯誤: {{ error.message }}</p>
      
      <div v-else class="notes-wall">
        <div 
          v-for="note in notes" 
          :key="note.id" 
          class="note"
          :style="{
            backgroundColor: note.color,
            left: `${getNotePosition(note).x}px`,
            top: `${getNotePosition(note).y}px`,
            zIndex: draggingNoteId === note.id ? 10 : 1
          }"
          @mousedown="startDrag(note.id, $event)"
        >
          {{ note.content }}
        </div>
      </div>

      <div class="mouse-position">
        滑鼠位置: X: {{ mouseX }}, Y: {{ mouseY }}
      </div>
    </div>
  </div>
</template>

<style scoped>
.notes-app {
  min-height: 100vh;
  padding: 20px;
  background-color: #f5f5f5;
  transition: background-color 0.3s;
}

.notes-app.dark-mode {
  background-color: #222;
  color: #eee;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.actions {
  display: flex;
  gap: 15px;
  align-items: center;
}

.mode-toggle {
  font-size: 24px;
  background: none;
  border: none;
  cursor: pointer;
}

.add-note-form {
  display: flex;
  gap: 10px;
}

.add-note-form input {
  padding: 8px 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.dark-mode .add-note-form input {
  background-color: #333;
  border-color: #444;
  color: #fff;
}

button {
  background-color: #4CAF50;
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}

.notes-container {
  position: relative;
  height: calc(100vh - 100px);
}

.notes-wall {
  position: relative;
  height: 100%;
  width: 100%;
  overflow: hidden;
}

.note {
  position: absolute;
  width: 200px;
  height: 200px;
  padding: 15px;
  box-shadow: 0 3px 6px rgba(0,0,0,0.16);
  cursor: move;
  border-radius: 4px;
  user-select: none;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 18px;
  text-align: center;
}

.mouse-position {
  position: fixed;
  bottom: 20px;
  right: 20px;
  background-color: rgba(255,255,255,0.7);
  padding: 5px 10px;
  border-radius: 4px;
  font-size: 14px;
}

.dark-mode .mouse-position {
  background-color: rgba(0,0,0,0.7);
  color: #fff;
}

.error {
  color: red;
}
</style>