<template lang="pug">
  #app
    mental-health
</template>

<script>
import MentalHealth from './components/mental-health-questionnaire.vue'

export default {
  name: 'App',
  components: {
    MentalHealth
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
