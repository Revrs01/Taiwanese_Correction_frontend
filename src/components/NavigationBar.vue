<script setup>
import NavigationBarIcon from '@/components/icons/NavigationBarIcon.vue'
</script>

<template>
  <div class="navbar">
    <div style="width: 3px"></div>
    <div class="navbar-svg list-toggle">
      <NavigationBarIcon style="width: 40px; height: 40px"/>
    </div>
    <div style="width: 12px"></div>
    <div class="navbar-block">
      成功大學 台灣文學系 校正網站
    </div>
  </div>

</template>

<style scoped>
.navbar {
  display: flex;
  flex-direction: row;
  width: 100vw;
  height: 15%;
  background-color: grey;
}

.navbar-svg {
  padding: 10px 3px 0;
  align-content: center;
  color: white;
  border: solid darkgrey;
  border-radius: 10px;
  margin: 4px;
}

.list-toggle:hover {
  background-color: dimgray;
  cursor: pointer;
}

.navbar-block {
  padding: 3px 0 3px;
  align-content: center;
  color: white;
  font-size: 36px;
}
</style>