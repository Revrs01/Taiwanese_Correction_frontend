<script>
import NavigationBarIcon from '@/components/icons/NavigationBarIcon.vue'
import PointLeftArrowIcon from '@/components/icons/PointLeftArrowIcon.vue'
import OffCanvasNavBar from '@/components/OffCanvasNavBar.vue'


export default {
  emits: ["back-to-filter-window", "is-side-nav-opened"],
  props: {
    navBarIcon: {
      type: String
    }
  },
  components: {
    OffCanvasNavBar,
    NavigationBarIcon,
    PointLeftArrowIcon
  },
  data() {
    return {
      isSideNavOpened: false
    }
  },
  methods: {
    clickNavButton() {
      if (this.navBarIcon === 'list') {
        this.isSideNavOpened = !this.isSideNavOpened
      } else {
        this.$emit('back-to-filter-window')
      }
    }

  },
  computed: {
    sideNavClass() {
      return this.isSideNavOpened && (this.navBarIcon === 'list') ? 'open-navbar' : ''
    }
  },
  watch: {
    navBarIcon(newVal) {
      if (newVal === "arrow") {
        this.isSideNavOpened = false
      }
    }
  }

}
</script>

<template>
  <div class="navbar">
    <div style="width: 3px"></div>
    <div class="navbar-svg list-toggle" @click="clickNavButton">
      <NavigationBarIcon
        v-if="navBarIcon === 'list'"
        class="icon" />
      <PointLeftArrowIcon
        v-if="navBarIcon === 'arrow'"
        class="icon" />
    </div>
    <div style="width: 12px"></div>
    <div class="navbar-block">
      成功大學 台灣文學系 校正網站
    </div>
  </div>
  <OffCanvasNavBar
    :class="sideNavClass"
  />
</template>

<style scoped>
.open-navbar {
  transform: translateX(200px);
}

.icon {
  width: 40px;
  height: 40px;
}

.navbar {
  display: flex;
  flex-direction: row;
  width: 100vw;
  height: 15%;
  background-color: grey;
  max-height: 70px;
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
  overflow-x: hidden;
}
</style>