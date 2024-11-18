<script>
export default {
  methods: {
    getVersionInfo(){
      alert("現在版本為: v2.0-beta2")
    },
    triggerDownload(){
      alert('下載功能尚未開放，請先使用校正功能')
    }
  }

}
</script>

<template>
  <div class="sidenav" ref="sideNav">
    <a href="#" @click="getVersionInfo">版本資訊</a>
    <a href="#" @click="triggerDownload">表單輸出</a>
  </div>
</template>

<style scoped>
.sidenav {
  height: 100%;
  width: 200px;
  position: fixed;
  z-index: -1;
  top: 0;
  left: -200px;
  background-color: rgba(211, 211, 211, 0.6);
  overflow-x: hidden;
  transition: 0.5s;
  padding-top: 80px;
  border-radius: 0 0 50px 0;
  border: thin solid dimgray;
}

.sidenav a {
  padding: 8px 8px 8px 32px;
  text-decoration: none;
  font-size: 25px;
  color: black;
  display: block;
  transition: 0.3s;
}

.sidenav a:hover {
  color: #f1f1f1;
  background-color: darkgray;
}

</style>