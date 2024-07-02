<script>

import { defineComponent } from 'vue'
import CorrectionBody from '@/components/CorrectionBody.vue'

export default defineComponent({
  components: { CorrectionBody },
  methods: {
    navOpen() {
      this.$refs.correctionTable.classList.toggle('push-right')
    },
    navClose() {
      this.$refs.correctionTable.classList.toggle('push-right')
    },
  }
})
</script>

<template>
  <div class="table-responsive" ref="correctionTable">
    <table class="table align-items-center table-flush " id="mainTable">
      <thead class="thead-light">
      <tr>
        <th scope="col" style="font-size: 18px;">題目</th>
        <th scope="col" style="font-size: 18px;">錄音檔</th>
        <!--        <th scope="col" style="font-size: 18px;">年級</th>-->
        <!--        <th scope="col" style="font-size: 18px;">班級</th>-->
        <!--        <th scope="col" style="font-size: 18px;">座號</th>-->
        <th scope="col" style="font-size: 18px;">評分</th>
        <!--        <th scope="col" style="font-size: 18px;">狀態</th>-->
        <!--        <th scope="col" style="font-size: 18px;">完成度</th>-->
        <!--        <th scope="col" style="font-size: 18px;">校正按鈕</th>-->
      </tr>
      </thead>
      <CorrectionBody />

    </table>
  </div>
</template>

<style>

</style>