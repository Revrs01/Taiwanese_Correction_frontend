<script>
import NavigationBar from '@/components/NavigationBar.vue'
import CorrectionMainWidget from '@/components/CorrectionMainWidget.vue'
export default {
  data() {
    return {
      currentCorrectionStudentInfo: {},
      displayIcon: "list",
      isSideNavOpened: null
    }
  },
  components: {
    // CorrectionTable,
    CorrectionMainWidget,
    NavigationBar
  },
  methods: {
    goToStudentTable() {
      this.displayIcon = "arrow"
    },
    backToFilterWindow() {
      this.displayIcon = "list"
    },

  }
}
</script>

<template>
  <main class="flex-col">
    <div class="margin-bottom top-layer">
      <NavigationBar
        :nav-bar-icon="displayIcon"
        @back-to-filter-window="backToFilterWindow"
      />
    </div>
    <CorrectionMainWidget
      :should-stop-correction="this.displayIcon === 'list'"
      @change-icon="goToStudentTable"
    />
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