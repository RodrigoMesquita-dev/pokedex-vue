<template>
    <div>
      <div v-if="!pokemon.status" >Selecione um pokemon</div>
      <div v-else >
        <span>Força:</span>
        <div class="progress mb-2">
            <div class="progress-bar" role="progressbar" :style="`width: ${pokemon?.status?.forca || null}%;`" :aria-valuenow="`${pokemon?.status?.forca || null}`" aria-valuemin="0" aria-valuemax="100">{{  pokemon?.status?.forca || null }}</div>
        </div>

        <span>Agilidade:</span>
        <div class="progress mb-2">
            <div class="progress-bar" role="progressbar" :style="`width: ${pokemon?.status?.agilidade || null}%;`" :aria-valuenow="`${pokemon?.status?.agilidade || null}`" aria-valuemin="0" aria-valuemax="100">{{ pokemon?.status?.agilidade || null }}</div>
        </div>

        <span>Ataque:</span>
        <div class="progress mb-2">
            <div class="progress-bar" role="progressbar" :style="`width: ${pokemon?.status?.ataque || null}%;`" :aria-valuenow="`${pokemon?.status?.ataque || null}`" aria-valuemin="0" aria-valuemax="100">{{ pokemon?.status?.ataque || null }}</div>
        </div>
        
        <span>Defesa:</span>
        <div class="progress mb-2">
            <div class="progress-bar" role="progressbar" :style="`width: ${pokemon?.status?.defesa || null}%;`" :aria-valuenow="`${pokemon?.status?.defesa || null}`" aria-valuemin="0" aria-valuemax="100">{{ pokemon?.status?.defesa || null }}</div>
        </div>
      </div>
    </div>
</template>

<script>
export default {
  name: 'Status',
  props: {
    pokemon: Object,
  }
}
</script>