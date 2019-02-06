<template >
  <!--v-if="Math.random() > 0.5"-->
  <div class="signin-popup" @click="close">
    <div class="modal-backdrop" role="dialog">
      <div class="modal" ref="modal">

        <div class="top-panel">
          <div class="close-modal" @click="close" aria-label="Close modal"></div>
        </div>

        <section class="modal-body">
            <div class="cost-items" v-if="categories">
              <div class="cost" v-for="cat in categories" @click.stop="toggleActive(cat.id)" :class="{ 'active': activeIndex === cat.id }">
                <i v-bind:class="cat.icon" v-bind:style="{'color': cat.color, 'font-size': '3em' }"></i>
                {{cat.name_ru}}
              </div>
            </div>

          <label for="amountField">Сумма</label>
          <input id="amountField" type="number" v-model="amount" name="amount">

          <label for="commentField">комментарий</label>
          <input id="commentField" type="text" v-model="comment" name="comment">

          <input type="submit" value="Добавить" @click="addCostPopup()">
        </section>

      </div>
    </div>

  </div>
</template>

<script>
  import axios from 'axios'

  export default {
    components: {
    },
    name: 'cost-popup',
    template: 'cost-popup',
    data() {
      return {
        activeIndex: undefined,
        amount: '',
        comment: ' ',
        categories: null,
        id: ''
      };
    },
    methods: {
      toggleActive: function(id){
        this.activeIndex = id;
        this.id = id;
      },
      addCostPopup (event) {
        this.$emit('close');
        this.$emit('addCost', this.id, this.amount, this.comment)
      },
      close(event) {
        if (event.target.className === 'signin-popup' || event.target.className === 'close-modal') {
          this.$emit('close');
        }
      },
      addIncomePopup (event) {
        this.$emit('close');
        this.$emit('addIncome', this.amount, this.comment)
      }
    },
    beforeCreate() {
      console.log('Nothing gets called before me!')

    },
    created() {
      let data = this.$store.state.data;

      if (data.error) {
        this.data.error = this.$store.state.data;
        if (data.error.status === 301){
          this.$router.push('/');
        }
      }
      if (data.response) {
        console.log(data.response)
        this.categories = data.response.result.categories;
      }

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


    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

  @import "../styles/abstracts/abst.module.scss";


  .modal-backdrop {
    background-color: white;
    width:30%;
    display: flex;
    justify-content: center;
    align-items: center;
  }


  input {
    margin: 10px;
    outline:none;

  }

  input[type="submit"] {
    width: 30%;
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

  input[type="submit"]:hover {
    background-color:#D41E35;
    -webkit-box-shadow:0 10px 20px 0 rgba(41,67,100,0.21);
    box-shadow:0 10px 20px 0 rgba(41,67,100,0.21);
    text-decoration:none
  }

  input[type="number"] {
    width: 40%;
    padding: 10px;
    font-size: 2.3em;

  }
  input[type="number"]:focus {
    outline: 0.2px solid #75787e;
  }


  label {
    font-weight: 600;
    color: #9499a2;
    letter-spacing: 0.3px;
    font-size: 0.9em;
    margin-bottom: -7px;
  }

  .cost-items {
    width: 90%;
    display: flex;
    justify-content: space-around;
    flex-wrap: wrap;

    .cost {
      cursor: pointer;
      margin-bottom: 10px;
      border: 1px solid #949494;
      border-radius: 0.3em;
      min-width: 130px;
      padding: 10px 0px 10px 0px;
      display: flex;
      align-items: center;
      flex-direction: column;
    }
  }
  .active {
    background-color: #e8e8e8;
  }



</style>
