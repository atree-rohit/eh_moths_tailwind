<template>
    <w-app>
        <w-toolbar shadow fixed>
            <div class="title2">Sikkim-Darjeeling Moths</div>
            <div class="spacer"></div>
            <span class="ml2">Item 1</span>
            <span class="ml2">Item 2</span>
            <span class="ml2">Item 3</span>
        </w-toolbar>
        <MothBook />

    </w-app>
</template>

<script>
// import Vue from 'vue'
import MothBook from './components/MothBook.vue'

// Vue.use(KeenUI);

export default {
  name: 'App',
  components: {
    MothBook
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  /* margin-top: 60px; */
}
</style>
