<template>
  <div class="fixed inset-0 -z-10 h-full w-full items-center px-5 py-24 [background:radial-gradient(125%_125%_at_50%_10%,#000_40%,#190134_100%)]"></div>
  <main lass="text-white py-10 sm:max-w-xl md:max-w-3xl lg:max-w-4xl xl:max-w-6xl mx-auto">
        <BaseComponent></BaseComponent>
  </main>
</template>

<script>
import BaseComponent from './components/BaseComponent.vue'

export default {
  name: 'App',
  components: {
    BaseComponent
  }
}
</script>

<style>
</style>
