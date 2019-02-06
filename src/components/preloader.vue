<template >
<div class="preloader">
  <div class="dots">
    <div class="dot-overtaking"></div>
  </div>
</div>
</template>

<script>

  export default {
    components: {
    },
    name: 'preloader',
    template: 'preloader',
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
        console.log(this.$el)
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
.preloader {

  transition: all 1s ease-in;
  z-index: 150;
  position:fixed;
  display:flex;
  width:100%;
  height:100%;
  justify-content:center;
  align-content: space-between;
  align-items: center;
  flex-direction: column;
  background:rgba(0,0,0,.8);
  .dots {
    width:40%;
    display: flex;
    justify-content: center;
    align-items: center;

    $dotWidth: 10px;
    $dotHeight: 10px;
    $dotRadius: $dotWidth/2;

    $dotColor: #FF4F65;
    $dotBgColor: $dotColor;
    $dotBeforeColor: $dotColor;
    $dotAfterColor: $dotColor;

    $dotSpacing: $dotWidth + $dotWidth/2;
    @mixin dot($width: $dotWidth, $height: $dotHeight, $radius: $dotRadius,$bgColor: $dotBgColor, $color: $dotColor ) {
      width: $width;
      height: $height;
      border-radius: $radius;
      background-color: $bgColor;
      color: $color;
    }
    /**
     * ==============================================
     * Experiment-Gooey Effect
     * Dot Overtaking
     * ==============================================
     */

    $dotColorHSL: #FF4F65;

    .dot-overtaking {
      /*top: 120px;*/
      /*left: 100px;*/
      position: relative;

      @include dot($width: 12px, $height: 12px, $radius: 6px, $bgColor: transparent, $color: $dotColorHSL);

      margin: -1px 0;
      box-shadow: 0 -20px 0 0;
      filter: blur(0.2px);
      animation: dotOvertaking 2s infinite cubic-bezier(.2, .6, .8, .2);

      &::before,
      &::after {
        content: '';
        display: inline-block;
        position: absolute;
        top: 0;
        left: 0;

        @include dot($width: 12px, $height: 12px, $radius: 6px, $bgColor: transparent, $color: $dotColorHSL);

        box-shadow: 0 -20px 0 0;
        filter: blur(0.2px);
      }

      &::before {
        animation: dotOvertaking 2s infinite cubic-bezier(.2, .6, .8, .2);
        animation-delay: .3s;
      }

      &::after {
        animation: dotOvertaking 1.5s infinite cubic-bezier(.2, .6, .8, .2);
        animation-delay: .6s;
      }
    }

    @keyframes dotOvertaking {
      0% {
        transform: rotateZ(0deg);
      }

      100% {
        transform: rotateZ(360deg);
      }
    }
  }
}




</style>
