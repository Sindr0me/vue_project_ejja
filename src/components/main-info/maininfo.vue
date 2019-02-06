<template >
  <div class="maininfo">
    <h1>О проекте</h1>
    <p><strong>Purse</strong> — это ваш бюджет домашнего хозяйства — совокупность всех доходов и расходов!</p>
    <p>С экономической точки зрения, под домашним хозяйством подразумевается группа лиц, чаще всего связанных родственными отношениями (семья) совместно принимающих экономические решения на основании совместного бюджета. Домашнее хозяйство может состоять из одного лица, самостоятельно и независимо формирующего свой бюджет.</p>
  </div>
</template>

<script>

  export default {
      name:"maininfo",
    data() {
      return {
      };
    },
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
