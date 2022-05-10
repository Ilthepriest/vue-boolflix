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
body{
  background-color:  #222;
}
*::-webkit-scrollbar-thumb {
    background-color: #222;
    border-radius: 10px;
    border: 2px solid red;
}
*::-webkit-scrollbar {
    height: 10px;
    width: 12px;
}
*::-webkit-scrollbar-track {
    background: #141414;
    border-radius: 10px;
}
</style>
