<template>
  <div class="layout-container">
    <header class="p-4 bg-gray-100 mb-4">
      <h1 class="text-2xl font-bold">我的 Nuxt 應用</h1>
    </header>
    <main class="container mx-auto p-4">
      <slot />
    </main>
    <footer class="p-4 bg-gray-100 mt-4 text-center text-gray-600">
      © 2025 測試 JSON Server 與 Vue Query
    </footer>
  </div>
</template>