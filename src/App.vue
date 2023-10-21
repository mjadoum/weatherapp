<template>
<nav class="navbar navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand">
      <img src="./assets/weatherLogo.png" alt="" width="80" height="74" class="d-inline-block align-text-center">
      Weather App - Group F
    </a>
  </div>
</nav>
  
  <Weather msg="Welcome to Your Vue.js App"/>

   
</template>

<script>
import Weather from "@/components/Weather.vue";

export default {
  name: 'App',
  components: {
    Weather
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Quicksand:wght@400;600&family=Tilt+Neon&display=swap');

#app {
  text-align: center;

}
.navbar{
height:70px;
padding-top:0px !important;
}
.navbar-brand{
font-family: 'Tilt Neon', sans-serif;
font-size:30px !important;
padding-top:0px !important;
}
 *,
html {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    }
    *,
*:before,
*:after {
	box-sizing: inherit;
}
</style>
