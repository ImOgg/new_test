# Vuetify Vue3 專案移植指南

這個文件整理了當前 Vuetify 專案的所有組件和功能，方便移植到 Nuxt3 專案中。

## 專案概述

這是一個基於 **Vue 3 + Vite + Vuetify 3** 的展示專案，包含多個功能組件和頁面範例。

## 核心技術棧

### 主要依賴
```json
{
  "@mdi/font": "^7.4.47",                    // Material Design Icons
  "@tanstack/vue-query": "^5.77.1",         // 資料查詢管理
  "dayjs": "^1.11.13",                       // 日期處理
  "sass": "^1.79.4",                         // CSS 預處理器
  "sass-loader": "^10.5.2",                 // Sass 載入器
  "vue": "^3.5.10",                          // Vue 3 框架
  "vue-router": "^4.5.1",                   // Vue 路由
  "vue-virtual-scroller": "^2.0.0-beta.8",  // 虛擬滾動組件
  "vuetify": "^3.7.0-beta.1"                // Vuetify UI 框架
}
```

### 開發依賴
```json
{
  "@vitejs/plugin-vue": "^5.1.4",
  "vite": "^5.4.8"
}
```

## Nuxt3 移植所需安裝的套件

```bash
# Vuetify 相關
npm install vuetify@next
npm install sass
npm install @mdi/font

# 其他功能套件
npm install dayjs
npm install vue-virtual-scroller@next
npm install @tanstack/vue-query

# Nuxt3 Vuetify 模組 (推薦)
npm install @nuxtjs/vuetify
```

## 專案結構

```
src/
├── components/
│   ├── HelloWorld.vue        # 基礎 Hello World 組件
│   └── TimePicker.vue        # 自定義日期選擇器組件
├── pages/
│   ├── Vuetify.vue          # Vuetify 組件展示頁面
│   ├── VirtualScroller.vue  # 虛擬滾動展示頁面
│   └── Trasation.vue        # CSS 動畫效果展示頁面
├── views/
│   ├── Home.vue             # 首頁
│   └── About.vue            # 關於頁面
├── assets/
│   └── vue.svg              # 靜態資源
├── App.vue                  # 根組件
├── main.js                  # 應用程式入口點
├── router.js                # 路由配置
└── style.css                # 全域樣式
```

## 核心配置

### 1. Vuetify 初始化配置 (main.js)

```javascript
import '@mdi/font/css/materialdesignicons.css';
import 'vuetify/styles';
import { createApp } from 'vue';
import { createVuetify } from 'vuetify';
import { aliases, mdi } from 'vuetify/iconsets/mdi';
import * as components from 'vuetify/components';
import * as directives from 'vuetify/directives';
import { zhHant, en } from 'vuetify/locale';
import { RecycleScroller } from 'vue-virtual-scroller';
import 'vue-virtual-scroller/dist/vue-virtual-scroller.css';

// 自定義繁體中文語言包
const customZhHant = {
  ...zhHant,
  datePicker: {
    ...zhHant.datePicker,
    header: '選擇日期',
  },
};

const vuetify = createVuetify({
  components,
  directives,
  locale: {
    locale: 'zhHant',
    fallback: 'en',
    messages: {
      zhHant: customZhHant,
      en,
    },
  },
});

// 全域註冊虛擬滾動組件
app.component('RecycleScroller', RecycleScroller);
```

### 2. 路由配置 (router.js)

```javascript
import { createRouter, createWebHistory } from 'vue-router'

const routes = [
  { path: '/', name: 'Home', component: () => import('./views/Home.vue') },
  { path: '/about', name: 'About', component: () => import('./views/About.vue') },
  { path: '/vuetify', name: 'vuetify', component: () => import('./pages/Vuetify.vue') },
  { path: '/transition', name: 'transition', component: () => import('./pages/Trasation.vue') },
  { path: '/virtual-scroller', name: 'virtual-scroller', component: () => import('./pages/VirtualScroller.vue') }
]
```

## 關鍵組件詳解

### 1. TimePicker 組件 (components/TimePicker.vue)

**功能**: 基於 Vuetify 的自定義日期選擇器組件  
**依賴**: `dayjs`  
**特色**:
- 支援 `v-model` 雙向綁定
- 自動格式化為 `YYYY-MM-DD` 格式
- 可自定義 label
- 使用 Material Design Icons

```vue
<template>
  <v-menu :close-on-content-click="true" location="bottom">
    <template v-slot:activator="{ props }">
      <v-text-field 
        v-model="formatDate" 
        v-bind="props" 
        readonly 
        append-inner-icon="mdi-calendar-month-outline"
        :label="label"
      ></v-text-field>
    </template>
    <v-date-picker v-model="datetime"></v-date-picker>
  </v-menu>
</template>
```

**Nuxt3 使用方式**:
```vue
<TimePicker v-model="form.birthdate" label="選擇日期" />
```

### 2. Vuetify 組件展示頁面 (pages/Vuetify.vue)

**展示的 Vuetify 組件**:
- `v-combobox` - 可輸入的下拉選單
- `v-autocomplete` - 自動完成輸入框
- `v-select` - 普通下拉選單
- `v-carousel` - 輪播圖
- `v-form` - 表單容器
- `v-text-field` - 文字輸入框
- `v-btn` - 按鈕
- `v-card` - 卡片
- `v-dialog` - 對話框
- `v-expansion-panels` - 手風琴面板
- `v-file-input` - 檔案上傳

**關鍵程式碼片段**:
```vue
<script setup>
import { ref } from 'vue';
import TimePicker from '../components/TimePicker.vue';

let form = ref({
  name: '',
  email: '',
  gender: '',
  birthdate: null,
});

let items = ref([
  { id: 1, name: 'California' },
  { id: 2, name: 'Colorado' },
  // ...更多項目
]);
</script>
```

### 3. 虛擬滾動展示 (pages/VirtualScroller.vue)

**功能**: 展示大量資料的虛擬滾動效果  
**依賴**: `vue-virtual-scroller`  
**效能**: 可處理 10,000+ 筆資料而不影響效能

```vue
<template>
  <RecycleScroller 
    :items="items" 
    :item-size="50" 
    key-field="id" 
    style="height: 80vh;"
    class="overflow-y-auto border"
  >
    <template #default="{ item }">
      <div class="p-2 border-b">{{ item.title }}</div>
    </template>
  </RecycleScroller>
</template>

<script setup>
const items = Array.from({ length: 10000 }, (_, i) => ({
  id: i + 1,
  title: `第 ${i + 1} 筆資料`
}))
</script>
```

### 4. CSS 動畫展示 (pages/Trasation.vue)

**包含的動畫效果**:
1. **點擊變換動畫** - 旋轉 + 縮放 + 顏色變化
2. **進場出場動畫** - 滑動淡入淡出效果
3. **列表動畫** - 添加/移除項目的過渡效果
4. **Keyframes 複雜動畫** - 結合多種效果的關鍵幀動畫

**動畫技術**:
- Vue 3 `<Transition>` 組件
- Vue 3 `<TransitionGroup>` 組件
- CSS `@keyframes` 自定義動畫
- `cubic-bezier` 緩動函數

## Nuxt3 移植步驟

### 1. 安裝 Nuxt3 Vuetify 模組

```bash
npm install @nuxtjs/vuetify
```

### 2. Nuxt 配置 (nuxt.config.ts)

```typescript
export default defineNuxtConfig({
  modules: ['@nuxtjs/vuetify'],
  vuetify: {
    theme: {
      defaultTheme: 'light'
    },
    locale: {
      locale: 'zhHant',
      fallback: 'en'
    }
  },
  css: [
    '@mdi/font/css/materialdesignicons.css',
    'vue-virtual-scroller/dist/vue-virtual-scroller.css'
  ]
})
```

### 3. 插件配置 (plugins/vuetify.client.ts)

```typescript
import { RecycleScroller } from 'vue-virtual-scroller'

export default defineNuxtPlugin((nuxtApp) => {
  nuxtApp.vueApp.component('RecycleScroller', RecycleScroller)
})
```

### 4. 頁面檔案移植

將 `pages/` 目錄下的檔案移動到 Nuxt3 的 `pages/` 目錄，並確保：
- 移除 `export default` 語句（Nuxt3 自動處理）
- 檢查 `<script setup>` 語法相容性
- 確保組件引入路徑正確

### 5. 組件移植

將 `components/` 目錄下的組件移植到 Nuxt3 的 `components/` 目錄，Nuxt3 會自動註冊組件。

## 注意事項

1. **TypeScript 支援**: TimePicker 組件使用了 TypeScript，確保 Nuxt3 專案已啟用 TypeScript 支援
2. **圖標字體**: 確保正確載入 `@mdi/font`
3. **虛擬滾動**: 需要設定容器高度，否則會出現錯誤
4. **語言包**: 自定義的繁體中文翻譯需要手動配置
5. **CSS 樣式**: 確保 Sass 預處理器正確配置

## 參考資源

- [Vuetify 3 官方文檔](https://vuetifyjs.com/)
- [Vue Virtual Scroller 文檔](https://github.com/Akryum/vue-virtual-scroller)
- [Day.js 文檔](https://day.js.org/)
- [時間選擇器參考資料](https://blog.csdn.net/qq_34361235/article/details/134715389)
- [虛擬滾動參考文章](https://ithelp.ithome.com.tw/articles/10326989)

## 結語

這個專案展示了 Vuetify 3 的核心功能和 Vue 3 的現代特性。移植到 Nuxt3 時，主要考慮模組化配置和 SSR 相容性。所有組件都採用 Composition API 編寫，易於維護和擴展。
