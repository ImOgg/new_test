<script setup>
import { ref, computed } from 'vue';
import TimePicker from '../components/TimePicker.vue'; // 引入 TimePicker 組件
let items = ref([]); // 定義一個可變的 items 列表

// 在 mount 時更新 items 的值
items.value = [
  { id: 1, name: 'California' },
  { id: 2, name: 'Colorado' },
  { id: 3, name: 'Florida' },
  { id: 4, name: 'Georgia' },
  { id: 5, name: 'Texas' },
  { id: 6, name: 'Wyoming' }
];
let form = ref({
  name: '',
  email: '',
  gender: '',
  birthdate: null,  // 設置為 null，避免使用空字符串
});
let selectedItem = ref(null); // 儲存選中的項目
const colors = ['primary', 'secondary', 'yellow darken-2', 'red', 'orange'];
const model = ref(0);
</script>
<template>
  <v-container>
    <!-- 可以輸入數值的select 注意如果資料是object 要用item-title 不然他會顯示object object-->
    <v-combobox label="Combobox" v-model="selectedItem" item-title="name" :items="items"></v-combobox>

    <v-autocomplete label="Autocomplete" v-model="selectedItem" item-title="name" :items="items"></v-autocomplete>
    <!-- 普通的select -->
    <v-select v-model="selectedItem" :items="items" label="Select a State" item-title="name" item-value="id"></v-select>
    <p>Selected item: {{ selectedItem }}</p> <!-- 顯示選中的 id 或完整的物件 -->
    <v-carousel v-model="model" show-arrows>
      <v-carousel-item v-for="(color, i) in colors" :key="color" :value="i">
        <v-sheet :color="color" height="100%" tile>
          <div class="d-flex fill-height justify-center align-center">
            <div class="text-h2">Slide {{ i + 1 }}</div>
          </div>
        </v-sheet>
      </v-carousel-item>
    </v-carousel>
    <v-form>
      <!-- 名稱輸入框 -->
      <v-text-field v-model="form.name" label="名稱" required></v-text-field>

      <!-- 電子郵件輸入框 -->
      <v-text-field v-model="form.email" label="電子郵件" type="email" required></v-text-field>

      <!-- 性別選擇框 -->
      <v-select v-model="form.gender" :items="['男', '女']" label="性別" required class="w-full"></v-select>

      <!-- 日期選擇框 -->

      <TimePicker v-model="form.birthdate" label="選擇日期" />

      <v-btn color="primary" type="submit">提交</v-btn>
      <v-btn>
        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24">
          <!-- 這裡放你的 SVG 路徑 -->
          <path d="M10 20v-6h4v6h5v-8h3L12 3 2 12h3v8z" />
        </svg>
        123
      </v-btn>

      <v-card text="魚問貓我有沒有機會"></v-card>

      <v-dialog max-width="500">
        <template v-slot:activator="{ props: activatorProps }">
          <v-btn v-bind="activatorProps" color="surface-variant" text="Open Dialog" variant="flat"></v-btn>
        </template>

        <template v-slot:default="{ isActive }">
          <v-card title="Dialog">
            <v-card-text>
              Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et
              dolore magna aliqua.
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn text="Close Dialog" @click="isActive.value = false"></v-btn>
            </v-card-actions>
          </v-card>
        </template>
      </v-dialog>
      <v-expansion-panels>
        <v-expansion-panel title="Title"
          text="Lorem ipsum dolor sit amet consectetur adipisicing elit. Commodi, ratione debitis quis est labore voluptatibus! Eaque cupiditate minima">
        </v-expansion-panel>
      </v-expansion-panels>
      <v-file-input clearable label="File input"></v-file-input>
    </v-form>
  </v-container>
</template>
