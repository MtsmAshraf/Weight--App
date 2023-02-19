<template>
  <div class="overlay"></div>
  <div>
    <button class="unit-btn" v-bind:class="isKgActive? 'active' : null" @click="inKgs = true, inLbs = false, isKgActive = true, isLbActive = false">kg</button> 
    <button class="unit-btn" v-bind:class="isLbActive? 'active' : null" @click="inKgs = false, inLbs = true, isKgActive = false, isLbActive = true">lb</button> 
  </div>
  <div v-show="inKgs" class="input">
    <h3>Enter your weight in Kg</h3>
    <input type="text" min="0" v-model.lazy="weight" autofocus>
    <span>Kg</span>
  </div>
  <div v-show="inLbs" class="input">
    <h3>Enter your weight in lb</h3>
    <input type="text" min="0" v-model.lazy="weightInLbs">
    <span>lbs</span>
  </div>
  <button class="done-btn" @click="doneMethod()">Done</button>
  <div class="compare">
    <div class="min">
      <h4>Min</h4>
      <div class="current" v-if="minWeightInKgs != null">
        <h2 class="weight" v-show="inKgs">{{ minWeightInKgs }}</h2>
        <h2 class="weight" v-show="inLbs">{{ Math.round(minWeightInKgs * 2.2) }}</h2>
        <h3 v-show="inKgs">Kg</h3>
        <h3 v-show="inLbs">lbs</h3>
        <span>{{ dataObject[(Object.keys(dataObject).length) - (this.weightsArray.indexOf(this.minWeightInKgs)) >= Object.keys(dataObject).length ? (Object.keys(dataObject).length - 1) : (Object.keys(dataObject).length) - (this.weightsArray.indexOf(this.minWeightInKgs)) ]["dayOfTheMonthIndex"] }}/{{ dataObject[(Object.keys(dataObject).length) - (this.weightsArray.indexOf(this.minWeightInKgs)) >= Object.keys(dataObject).length ? (Object.keys(dataObject).length - 1) : (Object.keys(dataObject).length) - (this.weightsArray.indexOf(this.minWeightInKgs)) ]["monthIndex"] + 1 }}/{{ dataObject[(Object.keys(dataObject).length) - (this.weightsArray.indexOf(this.minWeightInKgs)) >= Object.keys(dataObject).length ? (Object.keys(dataObject).length - 1) : (Object.keys(dataObject).length) - (this.weightsArray.indexOf(this.minWeightInKgs)) ]["year"] }} </span>
      </div>
    </div>
    <div class="latest">
      <h4>latest</h4>
      <div class="current" v-if="this.dataObject[Object.keys(this.dataObject).length - 1].weightInKgs != null">
        <h2 class="weight" v-show="inKgs">{{ this.dataObject[Object.keys(this.dataObject).length - 1].weightInKgs }}</h2>
        <h2 class="weight" v-show="inLbs">{{ Math.round(this.dataObject[Object.keys(this.dataObject).length - 1].weightInKgs * 2.2) }}</h2>
        <h3 v-show="inKgs">Kg</h3>
        <h3 v-show="inLbs">lbs</h3>
        <span>{{ dataObject[(Object.keys(dataObject).length) - (1)]["dayOfTheMonthIndex"] }}/{{ dataObject[(Object.keys(dataObject).length) - (1)]["monthIndex"] + 1 }}/{{ dataObject[(Object.keys(dataObject).length) - (1)]["year"] }} </span>
      </div>
    </div>
    <div class="max">
      <h4>Max</h4>
      <div class="current" v-if="maxWeightInKgs != null">
        <h2 class="weight" v-show="inKgs">{{ maxWeightInKgs }}</h2>
        <h2 class="weight" v-show="inLbs">{{ Math.round(maxWeightInKgs * 2.2) }}</h2>
        <h3 v-show="inKgs">Kg</h3>
        <h3 v-show="inLbs">lbs</h3>
        
        <span>{{ dataObject[(Object.keys(dataObject).length) - (this.weightsArray.indexOf(this.maxWeightInKgs)) >= Object.keys(dataObject).length ? (Object.keys(dataObject).length - 1) : (Object.keys(dataObject).length) - (this.weightsArray.indexOf(this.maxWeightInKgs))  ]["dayOfTheMonthIndex"] }}/{{ dataObject[(Object.keys(dataObject).length) - (this.weightsArray.indexOf(this.maxWeightInKgs)) >= Object.keys(dataObject).length ? (Object.keys(dataObject).length - 1) : (Object.keys(dataObject).length) - (this.weightsArray.indexOf(this.maxWeightInKgs))  ]["monthIndex"] + 1 }}/{{ dataObject[(Object.keys(dataObject).length) - (this.weightsArray.indexOf(this.maxWeightInKgs)) >= Object.keys(dataObject).length ? (Object.keys(dataObject).length - 1) : (Object.keys(dataObject).length) - (this.weightsArray.indexOf(this.maxWeightInKgs))  ]["year"] }} </span>
      </div>
    </div>
  </div>
  <div class="stats">
    <div>
      <button class="unit-btn" v-bind:class="showTable? 'active' : null" @click="showTable = true, showGraph = false">Table</button> 
      <button class="unit-btn" v-bind:class="showGraph? 'active' : null" @click="showTable = false, showGraph = true">Graph</button> 
    </div>
    <div class="visual">
      <div v-show="showTable" class="info-parent">
        <div v-if="Object.keys(dataObject).length >= 5">
          <div class="info-subparent" v-for="i in 5" :key='i'>
            <div class="info">
              <span>
                <span class="weight" v-show="inKgs">{{ dataObject[(Object.keys(dataObject).length) - (i)]["weightInKgs"] }} </span>
                <span class="weight" v-show="inLbs">{{ Math.round(dataObject[(Object.keys(dataObject).length) - (i)]["weightInKgs"] * 2.2) }} </span>
                <span v-show="inKgs">Kg</span>
                <span v-show="inLbs">lbs</span> - 
              </span>
              <span>{{ dataObject[(Object.keys(dataObject).length) - (i)]["dayOfTheMonthIndex"] }}/{{ dataObject[(Object.keys(dataObject).length) - (i)]["monthIndex"] + 1 }}/{{ dataObject[(Object.keys(dataObject).length) - (i)]["year"] }} </span>
              <span> - {{ dataObject[(Object.keys(dataObject).length) - (i)]["hours"] < 10 ? "0" + dataObject[(Object.keys(dataObject).length) - (i)]["hours"] : dataObject[(Object.keys(dataObject).length) - (i)]["hours"]}}:{{  dataObject[(Object.keys(dataObject).length) - (i)]["minutes"] < 10 ? "0" + dataObject[(Object.keys(dataObject).length) - (i)]["minutes"] : dataObject[(Object.keys(dataObject).length) - (i)]["minutes"] }}</span>
            </div>
        </div>
        </div>
        <div v-else-if="Object.keys(dataObject).length < 5">
          <div class="info-subparent" v-for="i in Object.keys(dataObject).length" :key='i'>
            <div class="info">
              <span>
                <span class="weight" v-show="inKgs">{{ dataObject[(Object.keys(dataObject).length) - (i)]["weightInKgs"] }} </span>
                <span class="weight" v-show="inLbs">{{ Math.round(dataObject[(Object.keys(dataObject).length) - (i)]["weightInKgs"] * 2.2) }} </span>
                <span v-show="inKgs">Kg</span>
                <span v-show="inLbs">lbs</span> - 
              </span>
              <span>{{ dataObject[(Object.keys(dataObject).length) - (i)]["dayOfTheMonthIndex"] }}/{{ dataObject[(Object.keys(dataObject).length) - (i)]["monthIndex"] + 1 }}/{{ dataObject[(Object.keys(dataObject).length) - (i)]["year"] }} </span>
              <span> - {{ dataObject[(Object.keys(dataObject).length) - (i)]["hours"] < 10 ? "0" + dataObject[(Object.keys(dataObject).length) - (i)]["hours"] : dataObject[(Object.keys(dataObject).length) - (i)]["hours"]}}:{{  dataObject[(Object.keys(dataObject).length) - (i)]["minutes"] < 10 ? "0" + dataObject[(Object.keys(dataObject).length) - (i)]["minutes"] : dataObject[(Object.keys(dataObject).length) - (i)]["minutes"] }}</span>
            </div>
          </div>
        </div>
      </div>
      <div v-show="showGraph" class="info-graph">
        <chartComponent/>
      </div>
    </div>
  </div>
  <div class="copy">
    &copy;By: <span><a href="https://moatasimashraf.000webhostapp.com" target="_blank">Moatasim</a></span>
  </div>
</template>

<script>
// import { JSON } from 'body-parser';
import chartComponent from "./components/chartComponent.vue";

export default {
  name: 'App',
  data(){
    return{
      weight:null,
      weightInLbs: this.weight == null ? 0 : Math.round(this.weight * 2.2), 
      inKgs:true,
      inLbs:false,
      isKgActive:true,
      isLbActive:false,
      dataObject:{},
      weightsArray:[],
      maxWeightInKgs:0,
      minWeightInKgs:0,
      daysArray:["Sun","Mon","Tue","Wed","Thu","Fri","Sat"],
      MonthsArray:["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul","Aug", "Sep", "Oct", "Nov", "Dec"],
      showTable:false,
      showGraph:true,
    }
  },
  components:{
    chartComponent,
  },
  methods:{
    doneMethod(){
      if(document.querySelector(".done-btn").value !== null && typeof(parseInt(document.querySelector(".done-btn").value)) == "number"){
        var dateNow = new Date();
        var dayOfTheMonthIndex = dateNow.getDate();
        var dayOfTheWeekIndex = dateNow.getDay();
        var monthIndex = dateNow.getMonth();
        var year = dateNow.getFullYear();
        // var month = this.MonthsArray[monthIndex];
        // var dayName = this.daysArray[dayOfTheWeekIndex];
        var hours = dateNow.getHours();
        var minutes = dateNow.getMinutes();
        this.dataObject[Object.keys(this.dataObject).length] = JSON.parse(`
        {
          "weightInKgs": ${this.weight},
          "year": ${year},
          "monthIndex": ${monthIndex},
          "dayOfTheMonthIndex": ${dayOfTheMonthIndex},
          "dayOfTheWeekIndex": ${dayOfTheWeekIndex},
          "hours": ${hours},
          "minutes": ${minutes}
        }
        `);
        window.localStorage.setItem("dataObject", JSON.stringify(this.dataObject))
      }
    }
  },
  watch:{
    weight(v){
          this.weightInLbs = Math.round(v * 2.2);
    },
    weightInLbs(v){
          this.weight = Math.round(v / 2.2);
    },
  },
  beforeMount(){
    if(window.localStorage.getItem("dataObject")){
        this.dataObject = JSON.parse(window.localStorage.getItem("dataObject"));
        // this.weight = this.dataObject[Object.keys(this.dataObject).length - 1].weightInKgs;
        for(let i = 0; i < Object.keys(this.dataObject).length; i++){
          this.weightsArray.push(this.dataObject[i].weightInKgs);
        }
        this.maxWeightInKgs = Math.max(...this.weightsArray);
        this.minWeightInKgs = Math.min(...this.weightsArray);
      }
  },
}
document.querySelectorAll(".unit-btn").forEach((btn)=>{
  btn.addEventListener("click",()=>{
    document.querySelectorAll(".unit-btn").forEach((btn)=>{
      btn.classList.remove("active");
    })
    btn.classList.add("active");
  })
})
</script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat+Alternates:wght@300;400;500;600;700&display=swap');
*{
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}
:root{
  --main-color:rgb(255, 37, 37);
  --sec-color:#220016;
  --sec-color-semi:#22001667;
  --main-color-semi:rgb(255,37,37,0.2);
  --light-color:white;
  --light-color-semi:rgba(255, 255, 255, 0.221);
  position: relative;
}
#app{
  /* font-family: Avenir, Helvetica, Arial, sans-serif; */
  /* -webkit-font-smoothing: antialiased; */
  /* -moz-osx-font-smoothing: grayscale; */
  text-align: center;
  color: var(--main-color);
  width: 450px;
  max-width: 90%;
  position: relative;
}
#app > *:not(.overlay){
  position: relative;
}
.overlay{
  position: fixed;
  width: 100vw;
  height: 100vh;
  left: 0;
  top: 0;
  background-color: var(--sec-color);
  opacity: 0.9;
}
button:focus{
  outline: none;
  cursor: pointer;
}
body{
  font-family: "Montserrat Alternates", sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  padding-top: 40px;
  padding-bottom: 40px;
  background-image: url(../img/bg.jpg);
  background-size: cover;
  background-position: center;
}
.unit-btn{
  padding: 5px 20px;
  border: none;
  border-top: 2px solid var(--light-color);
  border-bottom: 2px solid var(--light-color);
  background-color: transparent;
  cursor: pointer;
  color: var(--light-color);
  margin-bottom: 10px;
  transition: all 0.1s 0s linear;
}
.unit-btn:first-of-type{
  margin-right: 5px;
}
.unit-btn.active{
  background-color: var(--main-color-semi);
  color:var(--light-color);
  border: none;
  border-top: 2px solid var(--main-color);
  border-bottom: 2px solid var(--main-color);
}
.input{
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  /* width: 200px; */
  font-family: "Montserrat Alternates", sans-serif;
  height: 150px;
}
.input h3{
  text-align: center;
  font-size: 14px;
  font-weight: normal;
  margin-bottom: 10px;
  color: var(--light-color);
}
.input input{
  background-color: transparent;
  /* background-color: var(--light-color-semi); */
  border: none;
  color: var(--main-color);
  font-size: 40px;
  text-align: center;
  width: 150px;
  max-width: 100%;
  height: 60px;
  caret-color: var(--main-color);
  position: relative;
  padding: 0;
  margin: 10px auto;
  color: var(--light-color);
  border-top: 2px solid var(--main-color);
  border-bottom: 2px solid var(--main-color);
}
.input input:focus{
  color: var(--light-color);
  outline: none;
  transition: all 0.1s 0s linear;
  background-color: var(--main-color-semi);
}
.input span{
  color: var(--light-color);
  font-weight: bold;
}
.compare{
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 20px;
  margin: 20px auto 40px;
}
.compare > div{
  background-color: var(--light-color-semi);
  color: var(--light-color);
  padding: 20px;
  width: 150px;
  transform: scale(0.9);
  border-top: 2px solid var(--light-color);
  border-bottom: 2px solid var(--light-color);
  position:relative;
}
/* .compare > div::before{
  position:absolute;
  content:'';
  left:0;
  top:0;
  width:100%;
  height:100%;
  border: 2px solid var(--main-color);
} */
.compare > div.latest{
  background-color: var(--main-color-semi);
  border-top: 2px solid var(--main-color);
  border-bottom: 2px solid var(--main-color);
  transform: scale(1.2);
}
.compare > div.latest h2{
  color: var(--main-color);
}
.compare > div span{
  display: block;
  margin-top: 10px;
  font-size: 14px;
}
@media (max-width:500px) {
  .compare{
    gap:5px;
  }
  .compare > div{
    transform: scale(0.9);
    /* flex-direction: column; */
  }
  .compare > div.latest{
    transform: scale(1);
  }
  .compare > div h4,
  .compare > div h3{
    font-size: 14px;
  }
  .compare > div h2{
    font-size: 40px !important;
  } 
}
.stats{
  margin-top: 40px;
}
.visual{
  height: 400px;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  margin-top: 20px;
  padding: 20px;
  max-width: 100%;
}
.visual .info-parent{
  width: 90%;
  margin-top: 10px;
  margin-right: auto;
  margin-left: auto;
  border-top: 2px solid var(--light-color);
  border-bottom: 2px solid var(--light-color);
}
.visual .info-graph{
  width: 340px;
  background-color: var(--sec-color-semi);
}
.info-subparent:hover{
  background-color: var(--light-color-semi);  
}
.info-subparent:not(:last-child){
  border-bottom: 2px solid var(--light-color);
  transition: all 0.1s 0s linear;
  /* background: red; */
}
.info{
  padding: 5px 20px;
  color:var(--light-color);
  cursor: pointer;
}
.current .weight{
  font-size: 50px;
}
.info .weight{
  font-size: 20px;
  font-weight: 500;
}
.done-btn{
  padding: 5px 20px;
  border: none;
  border-top: 2px solid var(--light-color);
  border-bottom: 2px solid var(--light-color);
  background-color: var(--main-color-semi);
  cursor: pointer;
  color: var(--light-color);
  margin-bottom: 10px;
  transition: all 0.1s 0s linear;
  margin-bottom: 20px;
}
.done-btn:hover,
.done-btn:focus{
  border-color: var(--main-color);
}
.copy{
  color: var(--light-color);
  font-size: 14px;
}
.copy a{
  color: var(--main-color);
  text-decoration: none;
  text-transform: uppercase;
  font-weight: bold;
  font-size: 16px;
}
</style>
