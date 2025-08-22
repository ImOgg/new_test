<template>
  <v-container>

    <RecycleScroller :items="items" :item-size="50" key-field="id" style="height: 80vh;"
      class=" overflow-y-auto border">
      <template #default="{ item }">
        <div class="p-2 border-b">{{ item.title }}</div>
      </template>
    </RecycleScroller>
  </v-container>
</template>


<script setup>
import { RecycleScroller } from 'vue-virtual-scroller'
import 'vue-virtual-scroller/dist/vue-virtual-scroller.css';
const items = Array.from({ length: 10000 }, (_, i) => ({
  id: i + 1,
  title: `第 ${i + 1} 筆資料`
}))
</script>

<!-- 
參考文章 -> https://ithelp.ithome.com.tw/articles/10326989
方式一
1. 安裝 vue-virtual-scroller 和 @tanstack/vue-query
  npm install vue-virtual-scroller@next @tanstack/vue-query
2. 引入 vue-virtual-scroller 的樣式
  import 'vue-virtual-scroller/dist/vue-virtual-scroller.css';
3. 使用 RecycleScroller 組件
4. 設定 items 和 item-size
5. 使用 #default slot 來渲染每一項資料
6. 使用 key-field 來指定唯一鍵
7. 設定容器高度和樣式(這個沒設置就會出現錯誤)
方式二
1.在main.js中引入 RecycleScroller 組件
2. import { RecycleScroller } from 'vue-virtual-scroller'
3. import 'vue-virtual-scroller/dist/vue-virtual-scroller.css';
4. app.component('RecycleScroller', RecycleScroller)
-->