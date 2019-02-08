<template>
  <div class="profile">
    <!--прелоадер зависит от состояния в vuex хранилище-->
    <preloader v-if="this.$store.state.preloader === true"></preloader>

      <!--<p>данные : Результат: {{data.maindata.error}}</p>-->
    <div v-if="data.error.length > 0"> Вы не авторизированы или не зарегистрированы</div>
    {{getMainData}}

    <div v-if="Object.keys(data.maindata).length > 0">
      {{data.maindata}}
      <profileMain></profileMain>
    </div>
    <div v-if="Object.keys(data.maindata).length == 0">Данные не были получены
    </div>

  </div>

</template>

<script>
  import axios from 'axios'
  import preloader from '../components/preloader.vue'
  import profileMain from '../components/profile-main.vue'


  export default {
    components: {
        preloader,
        profileMain
    },
    name: 'profile',
    template: 'profile',
    data() {
      return {
        data: {
            maindata : {},
            error : {}
        }
      };
    },
    methods: {

    },
    beforeCreate() {


    },
    created() {
      this.$store.state.preloader = true;

      console.log('Nothing gets called before me!')
      axios
        .post('https://www.ejja.ru/api/', {
          "name": "getUserData",
          "param": {
          }
        }, {
          withCredentials: true,
          headers: {
            'Content-Type': 'application/json'
          }
        })
        .then(response => (this.$store.state.data = response.data))
        .catch(error => console.log(error))
        .finally(() => (
          this.$store.state.preloader = false
        ));
    },
    mounted () {

    },
    beforeDestroy () {
      console.log('beforeDestroy')
    },
    destroyed () {
      console.log('destroyed')
    },
    computed: {
      getMainData () {
        let data = this.$store.state.data;

        if (data.error) {
          this.data.error = this.$store.state.data;
          if (data.error.status === 301){
            setTimeout(()=>{
                this.$router.push('/');
            }, 5000);
            return 'Вы будете перенаправлены на глвную страницу, авторизируйтесь';
          }
        }
        if (data.response) {
          this.data.maindata = data.response;
        }
      }
    }
  }
</script>

<style lang="scss">
  @import "../styles/abstracts/abst.module.scss";

  .profile {
    width:100%;
    height:100%;
    /*margin: 0;*/
    /*padding: 0;*/
  }
</style>
