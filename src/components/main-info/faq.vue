<template >
  <div class="faq">
    <h1>FAQ</h1>
    <accord v-for="prop in data"
      :prop="prop"></accord>
  </div>
</template>

<script>
  import accord from '../accordion.vue'

  export default {
      components: {
        accord
      },
      name:"faq",
    data:
      function () {
        return {
          // поля с контентом аккордеона в FAQ
            data: {
              accorditems1: {head: 'Что это за проект?', body: '  Lorem ipsum dolor sit amet, consectetur adipisicing elit. Adipisci commodi dolorem, eos facere fugiat obcaecati perspiciatis repellat similique sint. Atque consequuntur deleniti eaque est eveniet ipsam mollitia placeat reprehenderit voluptate.'},
              accorditems2: {head: 'Какие проблемы решает?', body: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit. Adipisci commodi dolorem, eos facere fugiat obcaecati perspiciatis repellat similique sint. Atque consequuntur deleniti eaque est eveniet ipsam mollitia placeat reprehenderit voluptate.'}
            }
        }
      }
    ,
    methods: {
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

  @import "../../styles/abstracts/abst.module.scss";
  h1 {
    font-size: 1.6em;
  }
</style>
