<template>
  <q-page class="flex flex-center">
    <div class="column items-center">
      <h2>{{name}}</h2>
      <q-img :src="url" width="250px"/>
    </div>

    <div class="row justify-around full-width">
      <q-input filled v-model="search" label="Digite nome pokemon "  />
      <q-btn color="purple" label="Pesquisar" @click="getPokemon"/>
    </div>
    <div class="row justify-between absolute full-width container-arrows">
      <q-icon class="q-ml-sm cursor-pointer" name="far fa-solid fa-circle-arrow-left" color="primary" size="3rem" @click="getPokemon(id - 1)">
      <q-tooltip>Anterior</q-tooltip>
      </q-icon>
      <q-icon class="q-mr-sm cursor-pointer" name="far fa-solid fa-circle-arrow-right" color="primary" size="3rem" @click="getPokemon(id + 1)">
      <q-tooltip>Proximo</q-tooltip>
      </q-icon>
    </div>

    <font-awesome-icon icon="fa-solid fa-circle-arrow-right" />
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'
import api from "../services/api";

export default defineComponent({
  name: 'IndexPage',

  data(){
    return{
      name: "",
      url: "",
      search: 'ditto',
      id: ''
    }
  },

  async beforeMount() {
    await this.getPokemon();

  },

  methods: {
    getPokemon(id) {
      api
        .get(id >0 ? `/pokemon/${id}/`:`/pokemon/${this.search}/`)
        .then( (response) => {
          // handle success
          console.log(response);
          this.id = response.data.id;
          this.name = response.data.name;
          this.search = response.data.name;
          this.url = response.data.sprites.other.dream_world.front_default

        })
        .catch( (error) => {
          // handle error
          this.triggerNegative();
        })
        .then( () => {
          // always executed
        });
    },

    triggerPositive () {
      this.$q.notify({
        type: 'positive',
        position: "top",
        message: 'Pokemon encontrado;)'
      })
    },

    triggerNegative () {
      this.$q.notify({
        type: 'negative',
        position: "top",
        message: 'Ocorreu um erro, tente novamente!'
      })
    },
  }
})
</script>

<style scoped>

.container-arrows{

}

</style>
