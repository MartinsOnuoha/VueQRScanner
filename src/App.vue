<template>
  <div id="app">
    <img width="200" class="app__logo" alt="App logo" src="./assets/logo.png">
    <Scanner></Scanner>
    <Footer></Footer>
  </div>
</template>

<script lang="ts">
import '@/assets/scss/_app.scss';
import '@/assets/scss/_mobile.scss';
import { Component, Vue } from 'vue-property-decorator';
import Scanner from './components/Scanner/Scanner.vue';
import Footer from './components/Footer/Footer.vue';

@Component({
  components: {
    Scanner,
    Footer,
  },
})
export default class App extends Vue {}
</script>

<style lang="scss">
#app {
  font-family: 'VT323', monospace;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 150vh;
}
</style>
