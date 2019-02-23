<template>
  <div id="app">
    <!--<router-view/>-->
    <shopcar></shopcar>
  </div>
</template>

<script>
  import Index from '@/components/index'
  import shopcar from '@/components/shopcar'
export default {
  name: 'App',
  components:{
       Index,shopcar
  }
}

</script>

<style>

</style>
