<template>
    <div>
        <div v-if="!pokemon.id" >Selecione um pokemon</div>
        <div v-else >
          <p>Identificador: {{ pokemon.id }}</p>
          <p>Nome: {{ pokemon.nome }}</p>
          <p>Tipo: {{ pokemon.tipo }}</p>
        </div>
    </div>
</template>

<script>
export default {
  name: 'Sobre',
  props: {
    pokemon: Object,
  }
}
</script>