<template>
    <div>
        <h1>我的 D3.js 圖表</h1>
        <D3Charts />
    </div>
</template>

<script>
import D3Charts from '~/components/D3Charts.vue';

export default {
    components: {
        D3Charts,
    },
};
</script>