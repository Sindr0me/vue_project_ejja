<template>

  <div class="hist-item" :class="{ 'red': prop.type == 'cost', 'green': prop.type == 'income'}">
    <div class="hist-item__type">
      <i :class="{ 'fas fa-minus': prop.type == 'cost', 'fas fa-plus': prop.type == 'income'}"></i>
    </div>
    <div class="hist-item__amount">{{prop.amount}}</div>
    <div class="hist-item__name">{{prop.name_ru}} </div>
    <div class="hist-item__date">{{prop.dadd}}</div>
    <div class="hist-item__comment">{{prop.comment}}</div>
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

  .fa-minus {
    color: indianred;
  }
  .fa-plus {
    color: #90cd4e;

  }

.hist-item {
  width: 100%;
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  padding: 10px;

  .hist-item__amount {
    width: 10%;
  }
  .hist-item__name {
    width: 20%;

  }
  .hist-item__date {
    width: 20%;

  }
  .hist-item__comment {
    width: 20%;

  }
}

</style>
