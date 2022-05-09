<template>
  <div id="app">
    <SiteHeader />
    
  </div>
</template>

<script>
import SiteHeader from '@/components/HeaderComponent.vue'

export default {
  name: 'App',
  components: {
    SiteHeader
  }
}
</script>

<style lang="scss">
@import "@/assets/scss/style.scss";

</style>
