<script setup>

</script>

<template>
  <tbody>
  <tr>
    <th scope="row" class="th-block">阿拉花瓜阿拉花瓜阿拉花瓜</th>
    <!--        <th scope="row" class="th-block">6年5班 20號</th>-->
    <th scope="row" class="th-block">
      <audio controls style="display: block; margin: 0 auto;">
        <source src='data:audio/wav;base64,${audioString}' type="audio/wav">
      </audio>
    </th>
    <th class="th-block">
      <button class="btn" style="color: white; background-color: orange; justify-content: space-evenly"
              buttonIndex="${index}">1 正確
      </button>
      <button class="btn" style="color: white; background-color: orange; justify-content: space-evenly"
              buttonIndex="${index}">0 錯誤
      </button>
      <button class="btn" style="color: white; background-color: orange; justify-content: space-evenly"
              buttonIndex="${index}">2 不會
      </button>
      <button class="btn" style="color: white; background-color: orange; justify-content: space-evenly"
              buttonIndex="${index}">3 沒聲音
      </button>
      <button class="btn" style="color: white; background-color: orange; justify-content: space-evenly"
              buttonIndex="${index}">4 音檔錯誤
      </button>
    </th>

  </tr>

  </tbody>
</template>

<style scoped>

</style>