<template>
  <div id="app">

    <!--<div id="nav">-->
      <!--<router-link to="/">Home</router-link>-->
      <!--<router-link to="/about">About</router-link>-->
      <!--<router-link to="/FAQ">FAQ</router-link>-->
      <!--<router-link to="/profile">Profile</router-link>-->
    <!--</div>-->
    <router-view/>
  </div>
</template>

<script>
//  import home from './components/HelloWorld.vue'
//  import headertpl from './components/header.vue'

  export default {
    name: 'app',
    data() {
//      console.log(this)
      return {
        counter: 0
      };
    },
    beforeCreate() {
      console.log('Nothing gets called before me!')
    },
    computed: {
      propertyComputed() {
        console.log('I change when this.property changes.')
        return this.property
      }
    },
    created() {
      this.obj = {};
      this.obj.ggg = 'dsfsdfs fsd fsdfsd f';
      setInterval(() => {
        this.counter++
      }, 1000)
    },
    mounted() {
      console.log(this) // I'm text inside the component.
    }
  }

</script>

<style lang="scss">
  @import "styles/abstracts/abst.module.scss";
  @import "styles/style.scss";

</style>
