<template>
<use-weather />
</template>

<script>
import useWeather from './components/useWeather.vue'

export default {
  name: 'App',
  components: {
    useWeather
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
font-family: 'Nunito', sans-serif;
font-weight: bold;
}
html,
body {
  height: 100%;

}
</style>
