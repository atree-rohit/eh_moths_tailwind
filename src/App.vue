<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  /* margin-top: 60px; */
}
.menu-link:hover{
    cursor: pointer;
    background: #ccc;
}
</style>

<template>
    <w-app>
        <w-toolbar shadow fixed>
            <div class="title2">Sikkim-Darjeeling Moths</div>
            <div class="spacer"></div>
            <span class="ml2 pa2 menu-link" @click="menuSelect(1)">About</span>
            <span class="ml2 pa2 menu-link" @click="menuSelect(2)">All Species</span>
            <!-- <span class="ml2 pa2 menu-link" @click="menuSelect(3)">Taxonomic Hierrachy</span> -->
        </w-toolbar>
        <AllSpecies v-if="selectedPage == 2"/>
        <!-- <MothBook v-else-if="selectedPage == 3"/> -->

    </w-app>
</template>

<script>
// import Vue from 'vue'
// import MothBook from './components/MothBook.vue'
import AllSpecies from './components/AllSpecies.vue'

// Vue.use(KeenUI);

export default {
    name: 'App',
    components: {AllSpecies },
    data() {
        return {
            selectedPage:2
        }
    },
    methods: {
        menuSelect(p){
            this.selectedPage = p
        }
    }
}
</script>
