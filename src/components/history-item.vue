<template>

  <div class="hist-item">
    {{prop.name_ru}} |
    {{prop.amount}} |
    {{prop.dadd}}
  </div>

</template>

<script>

  export default {
      name:"historyItem",
    data() {
      return {
      };
    },
    props: ['prop'],
    methods: {

    },
    computed: {

    },
    beforeCreate() {

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
