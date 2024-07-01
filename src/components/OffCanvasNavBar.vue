<script>
export default {
  methods: {
    openNav() {
      this.$refs.sideNav.style.width = '20%'
      this.$refs.sideNav.style.marginLeft = '10%'
    },
    closeNav() {
      this.$refs.sideNav.style.width = '0'
      this.$refs.sideNav.style.marginLeft = '-10%'
    }
  }
}
</script>

<template>
  <div id="mySidenav" class="sidenav" ref="sideNav">
    <a href="#">About</a>
    <a href="#">Services</a>
    <a href="#">Clients</a>
    <a href="#">Contact</a>
  </div>
</template>

<style scoped>
.sidenav {
  height: 100%;
  width: 0;
  position: fixed;
  z-index: 0;
  top: 0;
  left: -10%;
  background-color: lightgray;
  overflow-x: hidden;
  transition: 0.5s;
  padding-top: 10%;
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