<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <a href="#/characters">Characters</a>
    <a href="#/table">See excel data</a>
  </div>
  <component :is="currentView" />
</template>

<script>
import CharactersComponent from './CharactersComponent.vue';
import TableComponent from './TableComponent.vue';

const routes = {
  '/characters': CharactersComponent,
  '/table': TableComponent
}

export default {
  data() {
    return {
      currentPath: window.location.hash
    }
  },
  computed: {
    currentView() {
      return routes[this.currentPath.slice(1) || '/'];
    }
  },
  mounted() {
    window.addEventListener('hashchange', () => {
      this.currentPath = window.location.hash;
    });
  },
  name: 'MainComponent',
  props: {
    msg: String
  }
}
</script>

<style scoped>
a {
  color: black;
  text-decoration: none;
  font-size: 16px;
  font-weight: bolder;
}
</style>