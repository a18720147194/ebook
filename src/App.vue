<template>
  <div id="app">
    <div id="nav">
      <span class="d">ABCDEFG</span>
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view/>
  </div>
</template>
<script>
import { mapGetters } from 'vuex'
export default {
  computed: {
    ...mapGetters(['test'])
  },
  mounted () {
    this.$store.dispatch('setTest', 9).then(() => {
      console.log(this.test)
    })
  }
}
document.addEventListener('DOMContentLoaded', () => {
  const html = document.querySelector('html')
  let fontSize = window.innerWidth / 10
  fontSize = fontSize > 50 ? 50 : fontSize
  html.style.fontSize = fontSize + 'px'
})
</script>

<style scoped>
  .d{
    font-family: 'Days One';
  }
</style>
