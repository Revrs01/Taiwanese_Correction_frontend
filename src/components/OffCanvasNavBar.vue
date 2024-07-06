<script>
export default {


}
</script>

<template>
  <div class="sidenav" ref="sideNav">
    <a href="#">About</a>
    <a href="#">Services</a>
    <a href="#">Clients</a>
    <a href="#">Contact</a>
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
}

</style>