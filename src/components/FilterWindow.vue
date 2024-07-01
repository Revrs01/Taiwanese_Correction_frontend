<script>
export default {
  methods: {
    navOpen() {
      this.$refs.filterWindowRoot.classList.toggle('push-right')
    },
    navClose() {
      this.$refs.filterWindowRoot.classList.toggle('push-right')
    }
  }
}
</script>

<template>
  <div class="window-block flex-col" ref="filterWindowRoot">
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
      <button type="button" class="select-block shorten-button-size button-color">送出篩選</button>
    </div>
  </div>
</template>

<style scoped>
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

.push-right {
  transform: translateX(20%);
}
</style>