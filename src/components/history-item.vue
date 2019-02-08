<template>

  <div class="hist-item">
    item
  </div>

</template>

<script>

  export default {
      name:"historyItem",
    data() {
      return {
      };
    },
    props: ['item'],
    methods: {

    },
    computed: {

    },
    beforeCreate() {
      console.log('Nothing gets called before me!')
    },
    created() {

    },
    mounted () {
    },
    beforeDestroy () {
      console.log('beforeDestroy')
    },
    destroyed () {
      console.log('destroyed')
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

  @import "../styles/abstracts/abst.module.scss";

</style>
