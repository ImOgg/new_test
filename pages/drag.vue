<template>
    <div>
        <h1>我的 Drag</h1>
        <draggable v-model="myArray" item-key="id" style="display: flex; flex-direction: column; gap: 20px;">
            <template #item="{ element }">
                <div>{{ element.name }}</div>
            </template>
        </draggable>
    </div>
</template>
<script>
    import draggable from 'vuedraggable'; //引入

    export default {
      components: {
        draggable
      },
      data() {
        return {
          myArray: [
            { name: 'John', id: 0 },
            { name: 'Joao', id: 1 },
            { name: 'Jean', id: 2 }
          ]
        };
      }
      //   ...
};
</script>