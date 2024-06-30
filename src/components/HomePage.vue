<script setup>
import NavigationBar from '@/components/NavigationBar.vue'
import FilterWindow from '@/components/FilterWindow.vue'
</script>

<template>
  <main class="flex-col">
    <NavigationBar class="margin-bottom"/>
    <FilterWindow />
  </main>
</template>

<style scoped>
.flex-col {
  display: flex;
  flex-direction: column;
  top: 0;
}

.margin-bottom {
  margin-bottom: 10vh;
}
</style>