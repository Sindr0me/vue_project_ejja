<template>
  <!--должен быть только 1 родительский элемент в шаблоне!!!-->
  <div>
    <transition name="modal-fade">
    <login v-if="isModalVisible" @close="closeModal" />
    </transition>

    <header class="header" >
      <div class="header__logo">
        <a href="index.html"><img height="34" width="130" src="../assets/img/icon2.svg" alt=""></a>
      </div>
      <div class="header__nav">
        <nav class="header__nav__nav">
          <a href="#">О проекте</a>
          <a href="#">FAQs</a>
          <a href="#">
            <button class="btn btn--sm" @click="showModal">Начать</button>
          </a>

        </nav>
      </div>
    </header>
  </div>
</template>

<script>
  //модалка авторизации и регистрации
  import login from './login.vue'

  export default {
    components: {
      login
    },
    data() {
      return {
        isModalVisible: false,
      }
    },
    methods: {
      showModal() {
        this.isModalVisible = true;
      },
      closeModal() {
        this.isModalVisible = false;
      }
    }
  }


</script>

<style scoped lang="scss">

  @import "../styles/abstracts/abst.module.scss";

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

        margin-top: 40px;

        A{
          margin: 0 20px;
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
          color: #a5aab2;
        }
      }
    }
  }
</style>

