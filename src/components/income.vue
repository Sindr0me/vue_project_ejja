<template >
  <div class="signin-popup" @click="close">
    <div class="modal-backdrop" role="dialog">
      <div class="modal" ref="modal">

        <div class="top-panel">
          <div class="close-modal" @click="close" aria-label="Close modal"></div>
        </div>

        <section class="modal-body">
          <label for="amountField">Сумма</label>
          <input id="amountField" type="number" v-model="amount" name="amount">

          <label for="commentField">комментарий</label>
          <input id="commentField" type="text" v-model="comment" name="comment">

          <input type="submit" value="Добавить" v-on:click="addIncomePopup">
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
    name: 'income-popup',
    template: 'income-popup',
    data() {
      return {
        amount: '',
        comment: ' '
      };
    },
    methods: {
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

</style>
