<template>
  <div class="fixed inset-0 -z-10 h-full w-full items-center px-5 py-24 [background:radial-gradient(125%_125%_at_50%_10%,#000_40%,#190134_100%)]"></div>
  <PokeCard></PokeCard>
</template>

<script>
import PokeCard from './components/PokeCard.vue'

export default {
  name: 'App',
  components: {
    PokeCard
  }
}
</script>

<style>
</style>
