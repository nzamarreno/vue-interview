<template>
  <div class="wrapper">
    <header class="header">
      <h1 class="header__text">{{ title }}</h1>
      <h2 class="header__text header__text--center">{{ counter }}</h2>
    </header>

    <ul>
      <label> Your search </label>
      <input class="input" type="text" v-model="search" />
      <li class="beer" v-for="n in filtered">
        <h2 class="beer__name" v-on:click="onClickName(n.name)">
          {{ n.name }}
        </h2>
        <h2 class="beer__tagline">{{ n.tagline }}</h2>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

interface Entity {
  id: number;
  name: string;
  tagline: string;
  image_url: string;
  brewers_tips: string;
  food_pairing: string[];
}

interface CounterDatas {
  counter: number;
  search: string;
  beers: Entity[];
}

export default defineComponent({
  name: "Counter",

  props: ['title'],

  data() {
    return {
      counter: 0,
      search: "",
      beers: [],
    } as CounterDatas;
  },

  mounted() {
    this.fetchBeers();

    setInterval(() => {
      this.counter++;
    }, 1000);
  },

  computed: {
    filtered(): any[] {
      if (!!this.beers.length && this.search !== "") {
        let searchResults = [];

        for (var index = 0; index < this.beers.length; index++) {
          const beerLowercase = this.beers[index].name.toLowerCase();

          if (beerLowercase.indexOf(this.search.toLowerCase()) >= 0) {
            searchResults.push(this.beers[index]);
          }
        }

        return searchResults;
      } 
      else {
        return this.beers;
      }
    },
  },

  methods: {
    fetchBeers() {
      fetch("https://api.punkapi.com/v2/beers")
        .then((response) => {
          if (response.ok) {
            return response.json();
          } else {
            throw new Error("There are beers");
          }
        })
        .then((response) => {
          this.beers = response;
        });
    },

    onClickName(beerName: any) {
      this.$emit("onClickName", beerName);
    },
  },
});
</script>

<style lang="scss">
$medium: 500;
.wrapper {
  padding: 0 2rem;
}

.header {
  &__text {
    &--center {
      text-align: center;
    }
  }
}

.input {
  width: 100%;
  box-sizing: border-box;
  border-radius: 0.4rem;
  margin-bottom: 2rem;
  border: 0.1rem solid black;
  height: 2.5rem;
  padding: 0 1rem;
}

.beer__title {
  cursor: pointer;
  font-size: 2rem;
}

.beer {
  border-bottom: 0.1rem solid black;
  padding-bottom: 1rem;
  margin-bottom: 1rem;

  &__tagline {
    font-size: 1.2rem;
    font-weight: $medium;
  }
}
</style>
