<script>
import NavigationBar from '@/components/NavigationBar.vue'
import FilterWindow from '@/components/FilterWindow.vue'
import OffCanvasNavBar from '@/components/OffCanvasNavBar.vue'

export default {
  components: {
    NavigationBar,
    FilterWindow,
    OffCanvasNavBar
  },
  methods: {
    openCloseOffCanvas(status) {
      if (status) {
        this.$refs.offCanvas.openNav()
        this.$refs.filterWindow.navOpen()
      } else {
        this.$refs.offCanvas.closeNav()
        this.$refs.filterWindow.navClose()
      }
    }
  }
}
</script>

<template>
  <main class="flex-col">
    <div class="margin-bottom top-layer">
      <NavigationBar ref="navigationBar" @trigger-off-canvas="openCloseOffCanvas" />
    </div>

    <OffCanvasNavBar ref="offCanvas" />
    <FilterWindow ref="filterWindow" />
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
  margin-bottom: 10vh;
}
</style>