<template>
  <h2 class="--text-center">User has clicked on: <br/>
  {{ selectedBeer }}</h2>
  <Counter @onClickName="onClick" title="Beer Interview aka prepare your 5@7" />

</template>

<script lang="ts">
import { defineComponent } from 'vue'
import Counter from './components/Counter.vue'

interface AppData {
  selectedBeer: string
}

export default defineComponent({
  name: 'App',

  components: {
    Counter
  },

  data() {
    return {
      selectedBeer: ''
    } as AppData
  },

  methods: {
    onClick(name: string) {
      this.selectedBeer = name;
    }
  }
  
})
</script>

<style>
html, body {
  font-family: "Poppins";
  font-size: 62,5%;
  padding: 0;
  margin: 0;
}

h1, h2, h3, h4, h5, h6 {
  padding: 0;
  margin: 0;
}

ul {
  padding: 0;
  margin: 0;
}

li {
  list-style: none;
}

.--text-center {
  text-align: center;
}
</style>
