<script>
export default {
  data() {
    return {
      isVisible: true
    }
  },
  methods: {
    navOpen() {
      this.$refs.filterWindowRoot.classList.toggle('push-right')
    },
    navClose() {
      this.$refs.filterWindowRoot.classList.toggle('push-right')
    },
    toggleVisibility() {
      this.isVisible = !this.isVisible

    }
  }
}
</script>

<template>
  <transition name="drag-up" @after-leave="() => this.$emit('filtered')">
    <div v-if="isVisible" class="window-block flex-col" ref="filterWindowRoot">
      <div class="col input-block">
        <div class="filter-title">學校名稱</div>
        <select class="select-block padding-start">
          <option value="" disabled selected>選擇學校</option>
          <option>OPTION 1</option>
        </select>
      </div>
      <div class="col input-block" style="padding: 1px 1px 0 1px">
        <div class="row padding-mid-lane">
          <div class="filter-title align-start input-block">年級</div>
          <div class="filter-title align-mid input-block">班級</div>
        </div>

        <div class="row">
          <select class="select-block shorten-width align-start padding-start">
            <option value="" disabled selected>選擇年級</option>
            <option>OPTION 1</option>
          </select>

          <select class="select-block shorten-width padding-start">
            <option value="" disabled selected>選擇班級</option>
            <option>OPTION 1</option>
          </select>
        </div>
      </div>
      <div class="row">
        <button type="button" class="select-block shorten-button-size button-color" @click="toggleVisibility">送出篩選
        </button>
      </div>
    </div>
  </transition>
</template>

<style scoped>
.drag-up-enter-active {
  transition: transform 0.6s, opacity 0.3s;
}

.drag-up-leave-active {
  transition: transform 0.6s, opacity 0.3s;
}

.drag-up-enter-from, .drag-up-leave-to {
  transform: translateY(-100%);
  opacity: 0;
}

.drag-up-leave {
  transform: translateY(0);
  opacity: 1;
}

.flex-col {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
}

.window-block {
  transition: 0.5s;
  width: 70vw;
  height: 50vh;
  display: block;
  background-color: lightgrey;
  align-self: center;
  border-radius: 20px;
  border: solid 10px;
  min-width: 400px;
  min-height: 300px;
}

.select-block {
  height: 40px;
  width: 100%;
  border-radius: 25px;
  font-size: 16px;
}

.shorten-width {
  width: 48%;
}

.align-start {
  flex: 0 0 auto;
}

.padding-start {
  padding: 10px;
}

.padding-mid-lane {
  padding: 10px 10px 0 0;
}

.align-mid {
  flex: 1;
  text-align: center;
}

.shorten-button-size {
  width: 98%;
}

.row {
  display: flex;
  flex-direction: row;
  justify-content: space-evenly;
}

.col {
  display: flex;
  flex-direction: column;
}

.filter-title {
  font-size: 28px;
  color: black;
}

.input-block {
  padding: 10px 8px 0 8px;
}

.button-color {
  background-color: #349e61;
  color: white;
  border: solid lightgray;
}

.button-color:hover {
  background-color: seagreen;
  cursor: pointer;
}

</style>