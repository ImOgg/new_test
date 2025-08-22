# JSON Server + VueUse + Nuxt 3 示例專案

此專案展示如何使用 **JSON Server** 模擬 API 後端，結合 **Nuxt 3**、**Vue Query**、**VueUse** 建立一個簡單且實用的前端應用。

## 功能特色

- **JSON Server 模擬 API**
  - 自定義 REST API 端點
  - 資料持久化儲存
  - 支援 GET、POST、PUT、DELETE 方法
- **VueUse 功能展示**
  - `useMouse`：追蹤滑鼠位置，實現便箋拖曳功能
  - `useStorage`：將便箋位置保存到本地儲存
  - `useColorMode`：實現明暗主題切換

## 快速開始

### 安裝依賴

```bash
npm install
```

### 啟動 JSON Server

```bash
npx json-server --watch db.json --port 3001
```

### 啟動 Nuxt 開發伺服器

```bash
npm run dev
```

## 專案結構

```plaintext
tttttttt/
├── app.vue                  # Nuxt 應用入口
├── db.json                  # JSON Server 資料檔案
├── layouts/
│   ├── default.vue          # 預設應用布局
├── pages/
│   ├── index.vue            # 文章列表頁面
│   └── page2.vue            # 便箋牆頁面
├── nuxt.config.ts           # Nuxt 配置檔案
└── package.json             # 專案依賴管理
```

## 主要檔案範例

### app.vue

```vue
<template>
  <div>
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
  </div>
</template>
```

### layouts/default.vue

```vue
<template>
  <div>
    <nav class="p-4 bg-gray-100 mb-4">
      <ul class="flex space-x-4">
        <li><NuxtLink to="/">文章列表</NuxtLink></li>
        <li><NuxtLink to="/page2">便箋牆</NuxtLink></li>
      </ul>
    </nav>

    <main class="container mx-auto p-4">
      <slot />
    </main>

    <footer class="p-4 bg-gray-100 mt-4 text-center text-gray-600">
      © 2025 JSON Server + VueUse 示例
    </footer>
  </div>
</template>

<style scoped>
nav {
  background-color: #f5f5f5;
}
nav a {
  color: #333;
  padding: 8px 16px;
  text-decoration: none;
  border-radius: 4px;
}
nav a:hover {
  background-color: #e0e0e0;
}
.router-link-active {
  font-weight: bold;
  background-color: #e0e0e0;
}
main {
  min-height: 70vh;
}
</style>
```

### layouts/default.vue

```vue
<template>
  <div class="default-layout">
    <slot />
  </div>
</template>

<style scoped>
.default-layout {
  padding: 20px;
}
</style>
```

## API 資料結構

### 文章（Posts）

```json
{
  "posts": [
    { "id": 1, "title": "第一篇文章", "content": "這是第一篇文章的內容" },
    { "id": 2, "title": "第二篇文章", "content": "這是第二篇文章的內容" },
    { "id": 3, "title": "第三篇文章", "content": "這是第三篇文章的內容" }
  ]
}
```

### 便箋（Notes）

```json
{
  "notes": [
    { "id": 1, "content": "買牛奶", "color": "#FFADAD", "x": 100, "y": 100 },
    { "id": 2, "content": "回覆郵件", "color": "#FFD6A5", "x": 350, "y": 150 },
    { "id": 3, "content": "週末去看電影", "color": "#FDFFB6", "x": 200, "y": 300 },
    { "id": 4, "content": "學習 Vue", "color": "#CAFFBF", "x": 450, "y": 250 }
  ]
}
```

## 技術筆記

### VueUse 亮點功能

- 狀態管理：`useStorage`, `useLocalStorage`, `useSessionStorage`
- 瀏覽器 API：`useMouse`, `useWindowSize`, `useGeolocation`, `useNetwork`
- 感測器：`useDeviceMotion`, `useDeviceOrientation`, `useBattery`
- UI 效果：`useColorMode`, `useFullscreen`, `usePreferredDark`
- 動畫與時間：`useRafFn`, `useTransition`, `useNow`, `useInterval`

### Vue Query 亮點功能

- 自動處理資料請求、緩存與背景更新
- 支援請求重試、去重、取消
- 樂觀 UI 更新（提升使用者體驗）
- 分頁與無限滾動支援

## 疑難排解

### 頁面找不到錯誤

- 確認 `app.vue` 有包含 `<NuxtPage />`
- `nuxt.config.ts` 未設定 `pages: false`
- 檔名大小寫正確

### JSON Server 連接問題

- 確認 JSON Server 運行於 `http://localhost:3001`
- `db.json` 格式正確
- 注意跨域（CORS）問題

1. **安裝 D3.js：**:
    - 。
        ```shell
          npm install d3
        ```
1. **安裝 vue.draggable**:
    - 參考 https://ithelp.ithome.com.tw/articles/10320500。
        ```shell
          npm i -S vuedraggable@next
        ```
    - 引入
        ```shell
          import draggable from 'vuedraggable'; 
        ```


## 延伸資源

- [Nuxt 3 官方文檔](https://nuxt.com/docs/getting-started/introduction)
- [VueUse 官方網站](https://vueuse.org/)
- [JSON Server GitHub](https://github.com/typicode/json-server)
- [Vue Query 文檔](https://tanstack.com/query/v4/docs/vue/overview)
- [Vue Query 文檔2](https://nuxt.com/modules/vue-query)


## 授權

本專案採用 MIT License。

