<template>
    <!-- 原有的過渡盒子 -->
    <div class="transition-box" :class="{ active: isActive }" @click="toggleActive">
      <slot>Click me to see transition effect</slot>
    </div>

    <!-- 進場出場動畫 -->
    <div class="controls">
      <button @click="showBox = !showBox">顯示/隱藏盒子</button>
    </div>
    
    <Transition name="slide-fade">
      <div v-if="showBox" class="animated-box">
        <p>這是一個帶有進場和出場動畫的盒子</p>
      </div>
    </Transition>

    <!-- 多個盒子的列表動畫 -->
    <div class="controls">
      <button @click="addItem">添加盒子</button>
      <button @click="removeItem">移除盒子</button>
    </div>

    <TransitionGroup name="list" tag="div" class="box-container">
      <div v-for="item in items" :key="item" class="list-item">
        {{ item }}
      </div>
    </TransitionGroup>

    <!-- 使用 Keyframes 的複雜動畫盒子 -->
    <div class="controls">
        <button @click="showKeyframeBox = !showKeyframeBox">切換 Keyframes 動畫盒子</button>
    </div>

    <Transition name="keyframe-animation">
        <div v-if="showKeyframeBox" class="keyframe-box">
            <div class="inner-circle"></div>
            <p>Keyframes 動畫效果</p>
        </div>
    </Transition>
</template>

<script setup>
import { ref } from 'vue';

// 原有的邏輯
const isActive = ref(false);
const toggleActive = () => {
    isActive.value = !isActive.value;
};

// 進場出場動畫的狀態
const showBox = ref(true);

// 列表動畫的狀態
const items = ref([1, 2, 3, 4, 5]);
const addItem = () => {
    items.value.push(items.value.length + 1);
};
const removeItem = () => {
    if (items.value.length > 0) {
        items.value.splice(Math.floor(Math.random() * items.value.length), 1);
    }
};

// Keyframes 動畫盒子的狀態
const showKeyframeBox = ref(true);
</script>

<style scoped>
/* 原有的樣式 */
.transition-box {
    width: 200px;
    height: 200px;
    margin: 2rem auto;
    background-color: #42b883;
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    border-radius: 8px;
    transition: all 0.5s cubic-bezier(0.68, -0.55, 0.27, 1.55);
    text-align: center;
    padding: 1rem;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.transition-box.active {
    transform: rotate(360deg) scale(1.2);
    background-color: #35495e;
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
    border-radius: 50%;
}

/* 控制按鈕的樣式 */
.controls {
    margin: 2rem auto;
    text-align: center;
}

button {
    background-color: #42b883;
    color: white;
    border: none;
    padding: 8px 16px;
    margin: 0 5px;
    border-radius: 4px;
    cursor: pointer;
    font-size: 14px;
}

button:hover {
    background-color: #35495e;
}

/* 進場出場動畫的樣式 */
.animated-box {
    width: 300px;
    height: 100px;
    margin: 1rem auto;
    background-color: #3498db;
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 8px;
    text-align: center;
    padding: 1rem;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

/* 進場和出場的過渡效果 */
.slide-fade-enter-active {
    transition: all 0.5s ease-out;
}

.slide-fade-leave-active {
    transition: all 0.5s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
    transform: translateX(20px);
    opacity: 0;
}

/* 列表動畫的樣式 */
.box-container {
    width: 80%;
    max-width: 600px;
    margin: 1rem auto;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
}

.list-item {
    width: 80px;
    height: 80px;
    display: inline-flex;
    margin: 10px;
    background-color: #e74c3c;
    color: white;
    font-size: 24px;
    align-items: center;
    justify-content: center;
    border-radius: 6px;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.2);
}

/* 列表的過渡效果 */
.list-enter-active,
.list-leave-active {
    transition: all 0.5s ease;
}

.list-enter-from {
    opacity: 0;
    transform: translateY(30px);
}

.list-leave-to {
    opacity: 0;
    transform: translateY(-30px);
}

.list-move {
    transition: transform 0.5s;
}

/* Keyframes 動畫盒子的樣式 */
.keyframe-box {
    position: relative;
    width: 200px;
    height: 200px;
    margin: 3rem auto;
    background: linear-gradient(45deg, #9b59b6, #3498db);
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    font-weight: bold;
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
    overflow: hidden;
    text-align: center;
}

.keyframe-box p {
    z-index: 2;
    position: relative;
    text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
}

.inner-circle {
    position: absolute;
    width: 80px;
    height: 80px;
    background: rgba(255, 255, 255, 0.2);
    border-radius: 50%;
    animation: pulse 2s infinite;
}

/* Keyframes 動畫進出場效果 */
.keyframe-animation-enter-active {
    animation: float-in 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94) both;
}

.keyframe-animation-leave-active {
    animation: float-out 0.8s cubic-bezier(0.55, 0.085, 0.68, 0.53) both;
}

@keyframes float-in {
    0% {
        transform: translateY(40px) scale(0.8);
        opacity: 0;
        box-shadow: 0 0 0 rgba(0, 0, 0, 0);
    }

    50% {
        opacity: 1;
    }

    100% {
        transform: translateY(0) scale(1);
        opacity: 1;
        box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
    }
}

@keyframes float-out {
    0% {
        transform: translateY(0) scale(1) rotateZ(0);
        opacity: 1;
    }

    100% {
        transform: translateY(-40px) scale(0.8) rotateZ(10deg);
        opacity: 0;
    }
}

@keyframes pulse {
    0% {
        transform: scale(1);
        opacity: 0.7;
    }

    50% {
        transform: scale(1.5);
        opacity: 0.3;
    }

    100% {
        transform: scale(1);
        opacity: 0.7;
    }
}
</style>