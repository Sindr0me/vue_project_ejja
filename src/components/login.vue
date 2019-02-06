<template >
  <!--v-if="Math.random() > 0.5"-->
  <div class="signin-popup" @click="close">
    <div class="modal-backdrop" role="dialog">
      <div class="modal" ref="modal">

        <div class="top-panel">
          <div class="close-modal" @click="close" aria-label="Close modal"></div>
        </div>

        <keep-alive>
          <component :is="currentTab.component" class="tab"></component>
        </keep-alive>

        <section class="modal-body">
          <slot name="body">

            <div class="loginlink" v-if="currentTab.name === 'Зарегистрироваться'">
              <a href="#"
                 v-bind:key="tabs[0].name"
                 v-bind:class="['tab-button', { active: currentTab.name === tabs[0].name }]"
                 v-on:click="currentTab = tabs[0]"
              >{{ tabs[0].name }}</a>
            </div>

            <div class="loginlink" v-if="currentTab.name === 'Уже зарегистрированы?'">
              Еще нет аккаунта?
              <a href="#"
                 v-bind:key="tabs[1].name"
                 v-bind:class="['tab-button', { active: currentTab.name === tabs[1].name }]"
                 v-on:click="currentTab = tabs[1]"
              >{{ tabs[1].name }}</a>
            </div>


          </slot>
        </section>

      </div>
    </div>

  </div>
</template>

<script>
  import authoriz from './authorization.vue'
  import registr from './registration.vue'

  export default {
    components: {
      authoriz,
      registr
    },
    name: 'signin-popup',
    template: 'signin-popup',
    data() {
      var tabs = [
        { name: 'Уже зарегистрированы?',
          component: authoriz
        },
        { name: 'Зарегистрироваться',
          component: registr
        }

      ];
      return {
        currentTab: tabs[0],
        tabs: tabs,
      };
    },
    methods: {
      showpre () {
        console.log(1111111111111111)
      },
      close(event) {
        if (event.target.className === 'signin-popup' || event.target.className === 'close-modal') {
          this.$emit('close');
        }
      }
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
    },
    computed: {
      currentTabComponent: function () {
        return this.currentTab.name.toLowerCase()
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

  @import "../styles/abstracts/abst.module.scss";
.signin-popup {

  transition: all 1s ease-in;
  z-index: 100;
  position:fixed;
  display:flex;
  width:100%;
  height:100%;
  justify-content:center;
  align-content: space-between;
  align-items: center;
  flex-direction: column;
  background:rgba(0,0,0,.7);

  .modal-backdrop {
    background-color: white;
    width:40%;
    display: flex;
    justify-content: center;
    align-items: center;

  .modal {
    display: flex;
    width: 100%;
    height: 100%;
    flex-direction: column;

    .top-panel {
      display: flex;
      flex-direction: row-reverse;

      .close-modal {
        margin: 5px;
        width: 25px;
        height: 25px;
        cursor: pointer;
      }

      .close-modal:before {
        display: inline-block;
        content: '';
        width: 15px;
        height: 15px;
        border-right: 2px solid black;
        position: relative;
        right: 0px;
        top: 0px;
        transform: rotate(45deg);
      }

      .close-modal:after {
        display: inline-block;
        content: '';
        width: 15px;
        height: 15px;
        border-left: 2px solid black;
        position: relative;
        right: -10px;
        bottom: 19px;
        transform: rotate(-45deg);
      }
    }

    .modal-body {
      display: flex;
      align-items: center;
      justify-content: center;
      color: #585b61;

      .loginlink {
        font-size: 0.8em;
        margin-bottom: 30px;

        A {
        }

        A:link {
          text-decoration: none;
          color: #a5aab2;
        }

        A:visited {
          text-decoration: none;
          color: #a5aab2;
        }

        A:active {
          text-decoration: none;
          color: #a5aab2;
        }

        A:hover {
          text-decoration: none;
          color: #757a82;

        }
      }

    }
  }
  }
}


  @media #{$mobile-max} {

    .signin-popup {

      .modal-backdrop {
        width: 90%;

        .loginlink {
          font-size: 0.8em;
          text-align: center;
        }
      }

    }
  }

  @media #{$tablet} {

    .signin-popup {

      .modal-backdrop {
        width: 70%;
      }

    }
  }


</style>
