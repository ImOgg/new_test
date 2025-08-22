<template>
    <!-- 如果需要選擇完數值後自動關閉日期框 需要將close-on-content-click 設置為true -->
    <v-menu :close-on-content-click="true" location="bottom">
        <template v-slot:activator="{ props }">
            <!--v-text-field 加了這兩個參數 variant="underlined" density="compact" 會改變文本框的外觀  -->
            <v-text-field v-model="formatDate" v-bind="props" readonly append-inner-icon="mdi-calendar-month-outline"
                :label="label"></v-text-field>
        </template>
        <v-date-picker v-model="datetime"></v-date-picker>
    </v-menu>
</template>

<script lang="ts" setup>
import { ref, watch } from 'vue';
import Dayjs from 'dayjs';

// datepicker 初始格式
const datetime = ref<Date | null>(null);
const formatDate = ref<string>('');

// 接收來自父組件的 `modelValue` 和 `label`
defineProps<{
    modelValue: string | null;
    label: string;
}>();

// 觸發事件給父組件
const emit = defineEmits(['update:modelValue']);

// 監聽日期變化，將日期轉換為 "YYYY-MM-DD" 格式，並觸發更新
watch(datetime, () => {
    formatDate.value = Dayjs(datetime.value).format('YYYY-MM-DD');
    emit('update:modelValue', formatDate.value);
});
</script>