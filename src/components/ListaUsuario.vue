<template>
  <h1>Lista de usuários</h1>

  <span v-for="(nome, i) in nomeSelecionados" :key="i">
    {{ nome }}{{ nomeSelecionados.length > 1 ? ', ' : '' }}
  </span>
  <div class="lista">
    <div v-for="item in listaPessoas" :key="item.id">
      <Usuario
        @selecionado="adicionarFavorito($event)"
        @removeSelecionado="removeFavorito($event)"
        :usuario="item"
        :isFavorito="isFavorito(item.id)"
      />
    </div>
  </div>
</template>

<script>
import Usuario from './Usuario.vue'
import { computed, ref } from 'vue'

import { usuarioStore } from '../store/usuario'
export default {
  components: {
    Usuario,
  },
  setup() {
    const selecionados = ref([])
    const store = usuarioStore()
    const listaPessoas = store.$state.listaPessoas

    const nomeSelecionados = computed(() => {
      return selecionados.value.map((x) => `${x.first_name} ${x.last_name}`)
    })

    return {
      listaPessoas,
      nomeSelecionados,
      isFavorito: store.isFavorito,
      adicionarFavorito: store.adicionaUsuario,
      removeFavorito: store.removeUsuario,
    }
  },
}
</script>

<style>
h1 {
  text-align: center;
}

.lista {
  display: grid;
  margin: 0 20px;
  grid-template-columns: repeat(4, minmax(200px, 400px));
  justify-content: center;
  align-items: center;
  gap: 20px;
}
</style>
