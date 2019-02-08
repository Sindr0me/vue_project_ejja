<template>

  <div class="accordion">
    <article class="message" :class="accordionClasses">
      <div class="message-header" @click="toggleAccordion">
        {{prop.head}}
    </div>
      <div class="message-body">
        <div class="message-content">
          {{prop.body}}
        </div>
      </div>
    </article>
  </div>

</template>

<script>

  export default {
      name:"accordion",
    data() {
      return {
        isOpen: false
      };
    },
    props: ['prop'],
    methods: {
      toggleAccordion: function() {
        this.isOpen = !this.isOpen;
      }
    },
    computed: {
      accordionClasses: function() {
        return {
          'is-closed': !this.isOpen,
          'is-primary': this.isOpen,
          'is-dark': !this.isOpen
        };
      }
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
.accordion {

  .message {
    /*border: 1px solid #FF4F65;*/
    border-radius: 10px;
    max-width: 500px;
    /*margin-left: auto;*/
    /*margin-right: auto;*/
    margin: 5px;
    padding: 2px 0 2px 10px;
    /*background-color: #CEFF4F;*/
  }

  .message-header {
    cursor: pointer;
    font-size: 1.3em;
  }

  .message-header:before {
    content:'';
    display: inline-block;
    width:12px;
    height: 12px;
    border-bottom: 3px solid black;
    border-right: 3px solid black;
    transform: rotate(-135deg);
    /*margin-bottom: 7px;*/
    transition: all 0.5s ease;
  }

  .message-body {
    font-size: 1em;
    padding: 0;
    max-height: 10em;
    overflow: hidden;
    transition: 0.3s ease all;
  }

  .is-closed .message-body {
    max-height: 0;
  }
  .is-closed .message-header {
    color: #838790;
  }
  .is-closed  .message-header:before {
    content:'';
    display: inline-block;
    width:12px;
    height: 12px;
    border-bottom: 3px solid #585b61;
    border-right: 3px solid #585b61;
    transform: rotate(45deg);
    margin-bottom: 7px;
    transition: all 0.5s ease;
  }

  .message-content {
    /*padding: 20px;*/
  }
}

</style>
