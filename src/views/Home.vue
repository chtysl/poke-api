<template>
  <div class="w-full flex justify-center">
    <input
      type="text"
      placeholder="Enter pokemon"
      class="mt-10 border-blue border-2 p-2"
      v-model="text"
    />
  </div>
  <div class="mt-10 p-4 flex flex-wrap justify-center">
    <div
      class="ml-4 text-2xl text-blue-400"
      v-for="(pokemon, idx) in filtredPokemon"
      :key="idx"
    >
      <router-link :to="`/about/${idx + 1}`">
        {{ pokemon.name }}
      </router-link>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

import { reactive, toRefs, computed } from "vue";

export default {
  name: "Home",
  setup() {
    const state = reactive({
      pokemons: [],
      urlIdLookups: {},
      text: "",
      filtredPokemon: computed(() => updatePokemon()),
    });

    function updatePokemon() {
      if (!state.text) {
        return [];
      }
      return state.pokemons.filter((pokemon) =>
        pokemon.name.includes(state.text)
      );
    }
    fetch("https://pokeapi.co/api/v2/pokemon?offset=0")
      .then((res) => res.json())
      .then((data) => {
        console.log(data);
        state.pokemons = data.results;
        state.urlIdLookups = data.results.reduce(
          (acc, cur, idx) => (acc = { ...acc, [cur.name]: idx + 1 }),
          {}
        );
      });
    return { ...toRefs(state) };
  },
};
</script>
