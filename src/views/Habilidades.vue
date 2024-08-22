<template>
    <div>
      <div v-if="!pokemon.id" >Selecione um pokemon</div>
      <div v-else >
        <table class="table text-white">
            <tbody>
                <tr v-for="(h, indice) in pokemon.habilidades" :key="indice">
                    <td>{{ h }}</td>
                    <td class="d-flex justify-content-end">
                        <button type="button" class="btn btn-danger btn-sm">x</button>
                    </td>
                </tr>
            </tbody>
        </table>
        <input 
          type="text"
          class="form-control"
          placeholder="Adicionar habilidade"
          v-model="habilidade"
          @keyup.enter="adicionarHabilidade()"
        >
      </div>
    </div>
</template>

<script>
export default {
  name: 'Habilidades',
  props: {
    pokemon: Object,
  },
  data: () => ({
    habilidade: '',
  }),
  methods: {
    adicionarHabilidade(){
      this.$emit('adicionarHabilidade', this.habilidade);
      this.habilidade = '';
    }
  }
}
</script>

<style scoped>
.table td {
    border: none;
}
</style>