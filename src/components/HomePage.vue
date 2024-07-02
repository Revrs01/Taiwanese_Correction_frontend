<script>
import NavigationBar from '@/components/NavigationBar.vue'
import FilterWindow from '@/components/FilterWindow.vue'
import OffCanvasNavBar from '@/components/OffCanvasNavBar.vue'
import StudentTable from '@/components/StudentTable.vue'
// import CorrectionTable from '@/components/CorrectionTable.vue'

export default {
  components: {
    // CorrectionTable,
    NavigationBar,
    FilterWindow,
    OffCanvasNavBar,
    StudentTable
  },
  methods: {
    openCloseOffCanvas(status) {
      if (status) {
        this.$refs.offCanvas.openNav()
        if (this.$refs.studentTable.$data.isVisible) {
          this.$refs.studentTable.navOpen()
        }
        // this.$refs.correctionTable.navOpen()
        if (this.$refs.filterWindow.$data.isVisible) {
          this.$refs.filterWindow.navOpen()
        }
      } else {
        this.$refs.offCanvas.closeNav()
        if (this.$refs.studentTable.$data.isVisible) {
          this.$refs.studentTable.navClose()
        }
        // this.$refs.correctionTable.navClose()
        if (this.$refs.filterWindow.$data.isVisible) {
          this.$refs.filterWindow.navClose()
        }
      }
    },
    showStudentTable() {
      this.$refs.studentTable.toggleVisibility()
    },
    test() {
      // this.$refs.filterWindow.$data.isVisible = true
      this.$refs.studentTable.$data.isVisible = false
    }
  }
}
</script>

<template>
  <main class="flex-col">
    <button type="button" @click="test">CLICK ME</button>
    <div class="margin-bottom top-layer">
      <NavigationBar ref="navigationBar" @trigger-off-canvas="openCloseOffCanvas" />
    </div>

    <OffCanvasNavBar ref="offCanvas" />
    <FilterWindow ref="filterWindow" @filtered="showStudentTable" />
    <StudentTable ref="studentTable" @closeStudentTable="() => {this.$refs.filterWindow.$data.isVisible = true}" />
    <!--    <CorrectionTable ref="correctionTable" />-->
  </main>

</template>

<style scoped>
.flex-col {
  display: flex;
  flex-direction: column;
  top: 0;
}


.top-layer {
  z-index: 10;
}


.margin-bottom {
  margin-bottom: 3vh;
}
</style>