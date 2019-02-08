<template>

  <main class="main">

    <transition name="modal-fade">
     <income v-if="isModalVisible" @close="closeModal" v-on:addIncome="addIncome" />
    </transition>

    <transition name="modal-fade">
     <cost v-if="isModalCostsVisible" @close="closeCostsModal" v-on:addCost="addCost" />
    </transition>

    <div class="incomes-costs">
      <div class="incomes-costs-val">
        <span class="total-income-wrap">Доход: <span class="total-income">{{this.getTotalIncomes()}}</span></span><br>
        <span class="total-cost-wrap">Расход: <span class="total-cost">{{this.getTotalCosts()}}</span></span>
      </div>

    </div>
    <div class="graph" id="graph"></div>

    <div class="progressbar" v-if="!dontShowBar">
      <div class="containerWrap" ref="containerWrap">
        <div id="container"></div>
        <span>Остаток {{getdifference()}} руб</span>
      </div>
    </div>

    <div class="other-wrap">

      <div class="markers-wrap" v-if="markers">
        <div class="marker" v-for="marker in markers" v-bind:style="{'background-color': marker.color }">{{marker.label}} {{marker.value}} р.</div>
      </div>

      <div class="add-buttons">
        <button class="btn btn--sm" @click="showCostsModal">Расход</button>
        <button class="btn btn--sm green" @click="showModal">Доход</button>
      </div>

      <div class="history">

      </div>
    </div>

  </main>

</template>

<script>

  import income from './income.vue'
  import cost from './cost.vue'
  import axios from 'axios'

  export default {
    components: {
      income,
      cost
    },
      name:"profile-main",
    data() {
      return {
        id: '',
        amount: '',
        markers: null,
        isModalVisible: false,
        isModalCostsVisible: false,
        categories:'',
        dontShowBar: null
      };
    },
    methods: {
      getdifference () {

          let data = this.$store.state.data.response.result;
          if (!data.costs.total && !data.incomes.total) {
              this.dontShowBar = true;
              return false;
          }

          if (!data.costs.total) data.costs.total = 0;
          if (!data.incomes.total) data.incomes.total = 0;
          return data.incomes.total - data.costs.total;
      },
      getWidth (ref) {
        return this.$refs[ref].clientWidth;
      },
      initProgressBar (){
        if (this.getdifference() == false) return '';

        var data = [parseInt(this.getTotalIncomes()), this.getTotalCosts()]; // here are the data values; v1 = total, v2 = current value
        d3.select("#container")[0][0].innerHTML = '';

        var chart = d3.select("#container").append("svg") // creating the svg object inside the container div
          .attr("class", "chartt")
          .attr("width", 100 + '%') // bar has a fixed width
          .attr("height", 35);




        var x = d3.scale.linear() // takes the fixed width and creates the percentage from the data values
          .domain([0, d3.max(data)])
          .range([0, this.getWidth('containerWrap')]);

        chart.selectAll("rect") // this is what actually creates the bars
          .data(data)
          .enter().append("rect")
          .attr("width", x)
          .attr("height", 30)
          .attr("rx", 12) // rounded corners
          .attr("ry", 12);


        chart.selectAll("text") // adding the text labels to the bar
          .data(data)
          .enter().append("text")
          .attr("x", x)
          .attr("y", 10) // y position of the text inside bar
          .attr("dx", -8) // padding-right
          .attr("dy", 10) // vertical-align: middle
          .attr("text-anchor", "end") // text-align: right
          .text(String);
      },
      showModal() {
        this.isModalVisible = true;
      },
      closeModal() {
        this.isModalVisible = false;
      },
      showCostsModal() {
        this.isModalCostsVisible = true;
      },
      closeCostsModal() {
        this.isModalCostsVisible = false;
      },
      initGraph () {
        if (this.getdifference() == false) return '';

        var svg = d3.select("#graph").append("svg").attr("class", "svgd").append("g");
        svg.append("g").attr("class", "slicess"); //shadow
        svg.append("g").attr("class", "slices");
        svg.append("g").attr("class", "labels");

        var width = document.body.clientWidth,
          height = document.body.clientHeight/2,
          radius = Math.min(width, height) / 2;

        var pie = d3.layout.pie().sort(null).value(function (d) {
          return d.value;
        });
        var arc = d3.svg.arc() //размеры
          .outerRadius(radius * 1).innerRadius(radius * 0.6);

        svg.attr("transform", "translate(" + width / 2 + "," + height / 1.9 + ")");

        var key = function key(d) {
          return d.data.label;
        };

        var slicess = svg.select(".slicess").selectAll("path.slicess").data(pie([{label: "shadow", value: 100}]), key);
        slicess.enter().insert("path").style("fill", function (d) {
          return '#544f55';
        }).attr("class", "slicess");

        slicess.transition().duration(1000).attrTween("d", function (d) {
          this._current = this._current || d;
          var interpolate = d3.interpolate(this._current, d);
          this._current = interpolate(0);
          return function (t) {
            return arc(interpolate(t));
          };
        });

        svg.select('.slicess').attr("transform", "translate(" + 10 + "," + -5 + ")");

        this.updateCurrDiagramm();

      },
      getTotalIncomes() {
        var data = this.$store.state.data.response;
        if (!data.result.incomes.total) return 0;
        return data.result.incomes.total;
      },
      getTotalCosts() {
        var data = this.$store.state.data.response;
        if (!data.result.costs.total) return 0;
        return data.result.costs.total;
      },
      addCost (id, amount, comment) {
        axios
          .post('https://www.ejja.ru/api/', {
            "name": "addCost",
            "param": {
              "id": id,
              "amount": amount,
              "comment": comment
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
            this.$store.state.preloader = false,
            this.updateCurrDiagramm(),
            this.initMarkers(),
              this.initProgressBar()

      ));
      },
    addIncome (amount,comment) {
      axios
        .post('https://www.ejja.ru/api/', {
          "name": "addIncome",
          "param": {
            "amount": amount,
            "comment": comment
          }
        }, {
          withCredentials: true,
          headers: {
            'Content-Type': 'application/json'
          }
        })
        .then(response => (this.$store.state.data = response.data))
        .catch(error => console.log(error))
        .finally(() => {
        this.initProgressBar();
        });
    },

      updateCurrDiagramm () {
        var svg = d3.select("#graph");
        var pie = d3.layout.pie().sort(null).value(function (d) {
          return d.value;
        });
        var width = document.body.clientWidth,
          height = document.body.clientHeight/2,
          radius = Math.min(width, height) / 2;
        var key = function key(d) {
          return d.data.label;
        };      var arc = d3.svg.arc() //размеры
          .outerRadius(radius * 1).innerRadius(radius * 0.6);

        //если в vuex есть данные

        var changeData = [];
          var arrNames = [],
            arrColors = [];

          //пушим цвета для диаграммы
          this.$store.state.data.response.result.diagram.forEach((item, num)=>{
            arrNames.push(item.name_en);
            arrColors.push(item.color);

            changeData.push({"label": item.name_en, "value": item.amount})
          });

        var newChangeData = {};
        var newChangeDataArr = [];
        for (var i = 0; i < changeData.length; i++) {
          var str = changeData[i].label;

          if (newChangeData.hasOwnProperty(changeData[i].label)) {
            newChangeData[changeData[i].label] += parseInt(changeData[i].value);
          }else {
            newChangeData[str] = parseInt(changeData[i].value);
          }
        }

        for(var dfd in newChangeData) {
          newChangeDataArr.push({"label": dfd, "value": newChangeData[dfd].toString()})
        }

          //добавляем цвета
          var color = d3.scale.ordinal().domain(arrNames).range(arrColors);

          /* ------- PIE SLICES -------*/
          var slice = svg.select(".slices").selectAll("path.slice").data(pie(newChangeDataArr), key);
          slice.enter().insert("path").style("fill", function (d) {
            return color(d.data.label);
          }).attr("class", "slice");

          slice.transition().duration(1000).attrTween("d", function (d) {
            this._current = this._current || d;
            var interpolate = d3.interpolate(this._current, d);
            this._current = interpolate(0);
            return function (t) {
              return arc(interpolate(t));
            };
          });
          slice.exit().remove();
      },
      initMarkers () {

        var changeData = [];
        var newChangeData = {};
        var newChangeDataColors = {};
        var newChangeDataArr = [];

        //пушим цвета для диаграммы
        this.$store.state.data.response.result.diagram.forEach((item, num)=>{

          changeData.push({"label": item.name_ru, "value": item.amount, "color": item.color})
        });

        for (var i = 0; i < changeData.length; i++) {
          var str = changeData[i].label;

          if (newChangeData.hasOwnProperty(changeData[i].label)) {
            newChangeData[changeData[i].label] += parseInt(changeData[i].value);
          }else {
            newChangeData[str] = parseInt(changeData[i].value);
            newChangeDataColors[str] = changeData[i].color;
          }
        }

        for(let dfd in newChangeData) {
          newChangeDataArr.push({"label": dfd, "value": newChangeData[dfd].toString(), "color":newChangeDataColors[dfd] })
        }

        this.markers = newChangeDataArr;
      }

    },
    computed: {
    },
    beforeCreate() {
    },
    created() {

    },
    mounted () {
    this.initGraph();
    this.initMarkers();
    this.initProgressBar();
    },
    beforeDestroy () {
    },
    destroyed () {
    }
  }


</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

  @import "../styles/abstracts/abst.module.scss";
  .modal-fade-enter,
  .modal-fade-leave-active {
    opacity: 0;
  }

  span {
    color: #544f55;
  }
  .modal-fade-enter-active,
  .modal-fade-leave-active {
    transition: opacity 0.3s ease;
  }

  .progressbar {
    display: flex;
    width: 100%;
    justify-content: center;
    align-items: center;

    .containerWrap {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      width: 40%;
      font-size: 1.1em;

      #container {
        width: 100%;
      }
    }
  }

  .incomes-costs {

    font-size: 1.2em;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    .incomes-costs-val {
      position: absolute;
      top: 23%;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;

    }

    .total-income-wrap {
      min-width: 140px;
      min-height: 35px;
    background: url(../assets/img/income.png) 100% 100% no-repeat;
      text-align: center;
vertical-align: middle;

    }
    .total-cost-wrap {
      min-width: 140px;
      min-height: 35px;
      background: url(../assets/img/cost.png) 100% 100% no-repeat;
      text-align: center;
    }

  }

  .btn:hover,.btn:focus{
    background-color:#D41E35;
    -webkit-box-shadow:0 10px 20px 0 rgba(41,67,100,0.21);
    box-shadow:0 10px 20px 0 rgba(41,67,100,0.21);
    text-decoration:none
  }



.other-wrap {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-top: 20px;


  .markers-wrap {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;

    flex-wrap: wrap;
    width: 70%;

    .marker {
      margin: 0.4em 0.7em 0.3em 0.7em ;
      cursor: pointer;
      color: #ffffff;
      display: inline-block;
      padding: 7px 12px 7px 15px;
      border-radius: 1em;
      box-shadow: 2px 2px 5px rgba(0,0,0,0.5);
    }
  }

  .add-buttons {
    width: 50%;
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;

    .btn{
      min-width: 200px;
      border: 2px solid #c4322d;
      color:#c4322d;
      background-color: #ffffff;
      font-weight:500;
      text-decoration:none;
      display:inline-block;
      -webkit-transition:all 0.3s ease;
      transition:all 0.3s ease;
      border-radius: 2em;
      cursor: pointer;
    }

    .btn:focus{
      outline: none;
    }

    .green{
      border: 2px solid #39c44b;
      color: #39c44b;
    }

    .btn--sm{
      margin: 20px;
      font-size: 1.6em;
      padding: 20px;
      font-weight:500;
    }
  }
}


  @media #{$tablet} {

    .other-wrap {
      .add-buttons {
        margin-top:20px;
        width: 55%;

        .btn--sm{
          margin: 6px;
          font-size: 1.5em;
          padding: 15px;
        }
      }
    }
  }
  @media #{$mobile-max} {

    .progressbar {
      display: flex;
      width: 100%;
      justify-content: center;
      align-items: center;

      .containerWrap {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        width: 90%;
        #container {
          width: 100%;
        }
      }
    }

    .other-wrap {

      .markers-wrap {
        width: 100%;

        .marker {
          font-size: 0.9em;
        }
      }
      .add-buttons {
        margin-top:20px;
        width: 100%;

        .btn--sm{
          margin: 6px;
          font-size: 1.5em;
          padding: 15px;
        }
      }
    }
  }


  @media #{$mobile} {
    .other-wrap {

      .markers-wrap {
        width: 100%;

        .marker {
          font-size: 0.9em;
        }
      }

      .add-buttons {
        width: 90%;

        flex-direction: column;


        .btn--sm{
          margin: 10px;
          font-size: 1.5em;
          padding: 15px;
        }
      }
    }
  }


</style>
