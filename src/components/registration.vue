<template >
 <div class="reg">
   <div class="reg__header">
     <h2>Регистрация</h2>
   </div>
   <div class="reg__body">
     <form action="">

       <label for="emailField">электронная почта</label>
       <input id="emailField" type="email" v-model="email" name="email">

       <label for="nameField">имя</label>
       <input id="nameField" type="text" v-model="name" name="name">

       <label for="passwordField">пароль</label>
       <input id="passwordField" type="password" v-model="password" name="password">

       <label for="passwordField2">повторите пароль</label>
       <input id="passwordField2" type="password" name="password2" v-model="password2">

       <div class="error" v-show="email &amp;&amp; !isEmailValid"><span>Некорректный email адрес</span><span class="ion-alert-circled"></span>
       </div>

       <div class="error" v-show="password2 &amp;&amp; !isPassValid"><span>Пароли не совпадают</span><span class="ion-alert-circled"></span>
       </div>
       <input type="submit" value="Зарегистрироваться" :disabled="!isEmailValid || !isPassValid"  @click.prevent="postReg">

     </form>
   </div>
 </div>
</template>

<script>
  import axios from 'axios'

  export default {
    name:"registration",
    data() {
      return {
        email: '',
        password: '',
        name: '',
        password2:''
      };
    },
    methods: {
      postReg (event) {

        this.$store.state.preloader = true;

        axios
          .post('https://www.ejja.ru/api/', {
            name: "addUser",
            param: {
              email: this.email,//admin3@mail.ru
              password: this.password, //admin1
              name: this.name
            }
          }, {
            headers: {
              'Content-Type': 'application/json'
            }
          })
          .then(response => (this.$store.state.response = response.data))
          .catch(error => console.log(error))
          .finally(() => (
            this.$store.state.preloader = false,
              this.$store.state.login = true,
              this.$router.push('/myprofile')
          ));
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
    },
    computed :{
      isEmailValid() {
        return /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/.test(this.email);
      },
      isPassValid() {
          if (this.password !== this.password2) return false;
          if (this.password === this.password2) return true;
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

  @import "../styles/abstracts/abst.module.scss";
  .reg {
    display:flex;
    flex-direction: column;

    h2 {
      color: #585b61;

    }
    .reg__header{
      display:flex;
      flex-direction: column;
      align-items: center;
    }

    .reg__body {
      display:flex;
      flex-direction: column;
      align-items: center;

      form {
        padding:5%;
        align-items: center;
        width: 40%;
        display:flex;
        flex-direction: column;

        input {
          margin: 10px;
          outline:none;

        }
        input[type="submit"] {
          width: 100%;
          background-color:#FF4F65;
          color:#fff;
          font-weight:500;
          text-decoration:none;
          padding:13px 40px;
          -webkit-border-radius:8px;
          border-radius:8px;
          display:inline-block;
          letter-spacing:0.67px;
          -webkit-transition:all 0.3s ease;
          -o-transition:all 0.3s ease;
          transition:all 0.3s ease;
          border:none;
          cursor: pointer;
        }

        input[type="submit"]:hover {
          outline: none;
          background-color:#D41E35;
          -webkit-box-shadow:0 10px 20px 0 rgba(41,67,100,0.21);
          box-shadow:0 10px 20px 0 rgba(41,67,100,0.21);
          text-decoration:none
        }

        input[type="text"] {
          width: 90%;
          padding: 10px;
        }

        input[type="text"]:focus {
          outline: 0.2px solid #75787e;
        }

        input[type="email"] {
          width: 90%;
          padding: 10px;
        }
        input[type="email"]:focus {
          outline: 0.2px solid #75787e;
        }

        input[type="password"] {
          width: 90%;
          padding: 10px;
        }
        input[type="password"]:focus {
          outline: 0.2px solid #75787e;
        }

        label {
          font-weight: 600;
          color: #9499a2;
          letter-spacing: 0.3px;
          font-size: 0.9em;
          margin-bottom: -7px;
        }
      }
    }
  }

  @media #{$mobile-max} {
    .reg {
      .reg__body {
        form {
          width: 85%;

          input[type="submit"] {
            font-size: 0.8em;
          }
        }
      }
    }
  }
</style>
