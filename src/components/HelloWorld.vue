<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <CharactersComponent />
  </div>
</template>

<script>
import CharactersComponent from './CharactersComponent.vue';

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  components: {
    CharactersComponent
  }
}
</script>