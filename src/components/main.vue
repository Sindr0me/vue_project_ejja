<template>
  <!--должен быть только 1 родительский элемент в шаблоне!!!-->
  <div>
    <!--прелоадер зависит от состояния в vuex хранилище-->
    <preloader v-if="this.$store.state.preloader === true"></preloader>

    <transition name="modal-fade">
      <keep-alive><login v-if="isModalVisible" @close="closeModal" /></keep-alive>
    </transition>

    <header class="header" >
      <div class="header__logo">
       <img height="34" width="130" src="../assets/img/icon2.svg" alt="">
      </div>
      <div class="header__nav">
        <nav class="header__nav__nav">
          <a
             v-for="tab in tabs"
             v-bind:key="tab.name"
             v-bind:class="['main-menu-btn', { active: currentTab.name === tab.name }]"
             v-on:click="currentTab = tab"
          >{{ tab.name }}</a>
          <a>
            <button class="btn btn--sm" @click="showModal">Начать</button>
          </a>

        </nav>
      </div>
    </header>

    <main class="main">
      <div class="main__leftside">
          <div class="main__leftside__content">
            <component :is="currentTab.component" class="tab"></component>
          </div>

      </div>

      <div class="main__rightside" id="#main__rightside">
        <div class="main__rightside__slogan">
          <div class="slogan-1">Мы — считаем,</div>
          <div class="slogan-2">экономишь — <span class="slogan-2-color">ты</span></div>
        </div>

        <img alt="Токен" src="../assets/img/token.svg" class="mainpagetoken-left">
        <img alt="Токен" src="../assets/img/token.svg" class="mainpagetoken-middle">
        <img alt="Токен" src="../assets/img/token.svg" class="mainpagetoken-right">

        <img alt="Монета" src="../assets/img/moneyS.png" class="mainpagetoken-moneyS">
        <img alt="Монета" src="../assets/img/moneyM.png" class="mainpagetoken-moneyM">
        <img alt="Монета" src="../assets/img/moneyL.png" class="mainpagetoken-moneyL">
      </div>
    </main>

  </div>
</template>

<script>
  //модалка авторизации и регистрации
  import login from './login.vue'
  import preloader from './preloader.vue'


  import faq from './main-info/faq.vue'
  import maininfo from './main-info/maininfo.vue'


  export default {
    components: {
      login,
      faq,
      maininfo,
      preloader
    },
    data() {

      var tabs = [
          { name: 'О проекте',
          component: maininfo
          },
          { name: 'FAQ',
          component: faq
          }
        ];

      return {
        currentTab: tabs[0],
        tabs: tabs,
        isModalVisible: false
      };
    },
    methods: {
      showPre () {
          console.log(123213123)
      },
      showModal() {
        this.isModalVisible = true;
      },
      closeModal() {
        this.isModalVisible = false;
      }
    },
    beforeCreate() {
      console.log('Nothing gets called before me!')
    },
    computed: {
      currentTabComponent: function () {
        return this.currentTab.toLowerCase()
      }
    },
    created() {

    },
    mounted () {
      var wrap = this.$el.querySelector('.main__rightside'),
        tokenLeft = wrap.querySelector('.mainpagetoken-left'),
        tokenRight = wrap.querySelector('.mainpagetoken-right'),
        tokenMiddle = wrap.querySelector('.mainpagetoken-middle');

      function animateToken(num, selector) {
        var token = selector,
          firstCurrPosition = token.getBoundingClientRect().top - 78.3,
          currPosition = token.getBoundingClientRect().top - 78.3,
          currPositionR = document.documentElement.clientWidth - token.getBoundingClientRect().left,
          reverse = false;

        setInterval(function () {
          if (currPosition >= firstCurrPosition + num || reverse === true) {
            reverse = true;
            currPosition = currPosition - 0.2;
            token.style.transform = ('translate( -' + currPositionR + 'px, ' + currPosition + 'px)');
          }

          if (currPosition <= firstCurrPosition || reverse === false) {
            reverse = false;
            currPosition = currPosition + 0.2;
            token.style.transform = ('translate( -' + currPositionR + 'px, ' + currPosition + 'px)');
          }
        }, 10);
      }

      animateToken(30, tokenLeft);
      animateToken(25, tokenRight);
      animateToken(45, tokenMiddle);

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

  .modal-backdrop {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.3);
    display: flex;
    z-index: 2;
    justify-content: center;
    align-items: center;
  }

  .modal-fade-enter,
  .modal-fade-leave-active {
    opacity: 0;
  }

  .modal-fade-enter-active,
  .modal-fade-leave-active {
    transition: opacity 0.3s ease;
  }


  .btn:hover,.btn:focus{
    background-color:#D41E35;
    -webkit-box-shadow:0 10px 20px 0 rgba(41,67,100,0.21);
    box-shadow:0 10px 20px 0 rgba(41,67,100,0.21);
    text-decoration:none
  }

  .btn{
    background-color:#FF4F65;
    color:#fff;
    font-weight:500;
    text-decoration:none;
    padding:13px 40px;
    border-radius:8px;
    display:inline-block;
    letter-spacing:0.67px;
    -webkit-transition:all 0.3s ease;
    transition:all 0.3s ease;
    border:none;
    cursor: pointer;
  }

  .btn--sm{
    font-size:.875em;
    line-height:1.21em;
    padding:11px 24px;
    border-radius:4px;
    font-weight:500;
    letter-spacing:0.5px;
  }

  .header {
    display: flex;
    justify-content: space-between;

    .header__logo {
      margin-left: 80px;
      margin-top: 40px;
      a {
        padding: 20px;
      }
    }

    .header__nav {
      margin-right: 80px;

      .header__nav__nav{
        display: flex;
        justify-content: flex-end;
        align-items: baseline;

        margin-top: 40px;

        A{
          margin: 0 20px;
          color: #a5aab2;
          cursor: pointer;
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

  .main {
    display: flex;
    justify-content: space-between;

    .main__leftside{
      width: 50%;
    }

    .main__rightside {
      height: 600px;

      .main__rightside__slogan {
        font-size: 2.3em;
        color: #a5aab2;
        letter-spacing:3px;
        line-height:1.5em;
        position: absolute;
        top: 250px;
        right:270px;

        .slogan-1 {

        }

        .slogan-2 {
          font-size: 1.4em;

          .slogan-2-color  {
            color: #585b61;
            text-decoration: underline;
          }
        }
      }

      //стили монет
      .mainpagetoken-moneyL {
        position:absolute;
        top: 380px;
        right:300px;
      }

      .mainpagetoken-moneyM {
        position:absolute;
        top: 500px;
        right:240px;
      }

      .mainpagetoken-moneyS {
        position:absolute;
        top: 520px;
        right:380px;
      }

      //стили летающих токенов
      .mainpagetoken-left {
        position:absolute;
        transform: translate(-650px, 400px);
      }

      .mainpagetoken-middle {
        position:absolute;
        transform: translate(-420px, 120px);

      }

      .mainpagetoken-right {
        position:absolute;
        transform: translate(-220px, 220px);

      }
    }

    .main__leftside {
      .main__leftside__content {
        padding: 8% 10% 0 10%;
        line-height: 2;
        color: #585b61;
        font-size: 1.5em;

      }
    }
  }

  @media #{$tablet} {
    .main {
      .main__leftside {
        width: 100%;
      }
      .main__rightside {
        width: 0%;
        display: none;
      }
    }
  }


  @media #{$mobile-max} {
    .header {
      display: flex;
      flex-direction: column;
      align-items: center;

      .header__logo {
        display: flex;
        flex-direction: column;
        margin-left: 0px;
        margin-top: 40px;
        a {
          padding: 0px;
        }
      }

      .header__nav {
        margin-right: 0px;

        .header__nav__nav{
          display: flex;
          justify-content: flex-end;
          align-items: baseline;

          margin-top: 40px;

          A{
            margin: 0 15px;
            color: #a5aab2;
            cursor: pointer;
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

    .main {
      .main__leftside {
        width: 100%;
      }
      .main__rightside {
        width: 0%;
        display: none;
      }
    }
  }

</style>
