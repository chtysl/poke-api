<template>
  <div
    v-if="pokemon"
    class="w-3/12 m-auto bg-purple max-w-sm p-5 rounded mt-4 shadow-2xl flex justify-center flex-col items-center"
  >
    <h3 class="text-2xl text-green-900 uppercase">{{ pokemon.name }}</h3>

    <div class="flex justify-center">
      <img class="w-48" :src="pokemon.sprites.front_shiny" alt="" />
      <img class="w-48" :src="pokemon.sprites.back_shiny" alt="" />
    </div>

    <h3 class="text-yellow">Types</h3>

    <div v-for="(type, idx) in pokemon.types" :key="idx">
      <h5 class="text-blue">{{ type.type.name }}</h5>
    </div>
  </div>
</template>

<script>
import { useRoute } from "vue-router";
import { reactive, toRefs } from "vue";
export default {
  setup() {
    const route = useRoute();

    const state = reactive({
      pokemon: null,
    });

    async function getOnePokemon() {
      const res = await fetch(
        `https://pokeapi.co/api/v2/pokemon/${route.params.slug}/`
      );
      const data = await res.json();

      state.pokemon = data;
    }

    getOnePokemon();

    return { ...toRefs(state) };
  },
};
</script>

