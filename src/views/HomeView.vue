<template>
  <div class="home">
    <main>
      <section class="weather-app container">
        <div class="row">
          <div class="col-lg-12">
            <div class="d-flex">
          <h2 class="INSTAWEATHER">INSTAWEATHER</h2>
          <div class="span">
            <span>C</span>
            <span class="f">F</span>
          </div>
            </div>
          </div>
          <div class="row text-center m">
          <div class="col-lg-6">
            <h3>{{timeZone}}</h3>
             <input type="date" v-model="date">
             <h4 class='bx bx-sun'></h4>
             <h6>{{daily.data[5].icon}}</h6>
          </div>
          <div class="col-lg-6">
            
            <h5>{{currently.apparentTemperature}}°</h5>
            <h5>{{daily.data[5].temperatureMin}}°/{{daily.data[0].temperatureMax}}°</h5>
            <p>{{daily.summary}}</p>
          </div>
          </div>
          <div class="col-lg-12">
            <ul>
              <li @click="ShowHourlyMet()">Hourly</li>
              <li @click="ShowDailyMet()">Daily</li>
              <hr>
            </ul>
          </div>
          <div class="container">
          <div v-if="HideC">
         <div class="row" v-if="ShowDaily">
           <div class="col" v-for="(day , index) in daily.data" :key="index" >
            {{ new Date(day.time * 1000 ).getHours() + " : " + new Date(day.time * 1000 ).getMinutes() }}
            <p>{{day.apparentTemperatureHigh}}</p>
             </div>
          </div>
          <div class="row" v-if="ShowHourly">
           <div class="col" v-for="(hou , index) in hourly" :key="index" >
            {{ new Date(hou.time * 1000 ).getHours() + " : " + new Date(hou.time * 1000 ).getMinutes() }}
            <p>{{hou.temperature}}</p>
             </div>
          </div>
           </div>  
           <div v-if="HideF">
         <div class="row" v-if="ShowDaily">
           <div class="col" v-for="(day , index) in daily.data" :key="index" >
            {{ new Date(day.time * 1000 ).getHours() + " : " + new Date(day.time * 1000 ).getMinutes() }}
            <p>{{day.apparentTemperatureHigh * 1000}}</p>
             </div>
          </div>
          <div class="row" v-if="ShowHourly">
           <div class="col" v-for="(hou , index) in hourly" :key="index" >
            {{ new Date(hou.time * 1000 ).getHours() + " : " + new Date(hou.time * 1000 ).getMinutes() }}
            <p>{{hou.temperature}}</p>
             </div>
          </div>
           </div>        
         </div>
        </div>
      </section>
    </main>
  </div>
</template>

<script>
// @ is an alias to /src

import axios from "axios";
export default {
  name: "HomeView",
  data() {
    return {
      ShowHourly:true,
      ShowDaily:false,
      currently: null,
      daily : null,
      hourly :null,
      timeZone :'',
      date: new Date().toISOString().substr(0, 10)
    };
  },
  created() {
    axios
      .get("https://api.darksky.net/forecast/a177f8481c31fa96c3f95ad4f4f84610/29.345205,31.2084894")
      .then((res) => {
        console.log(res);
       this.currently =  res.data.currently
       this.daily = res.data.daily
       this.hourly  = res.data.hourly.data.slice(0,9);
       this.timeZone = res.data.timezone
      })
  },
  methods:{
    ShowHourlyMet(){
      this.ShowHourly = true
      this.ShowDaily = false
    },
    ShowDailyMet(){
      this.ShowHourly = false
      this.ShowDaily = true
    },
  }
}
</script>

<style lang="scss" scoped>
@import url('https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;700&family=Work+Sans:wght@400;500;600&display=swap');
main {
  background-image: url("@/assets/back.jpg");
  background-size: cover;
  width: 100%;
  height: 600px;
    font-family: "Work Sans";
    font-style: normal;
    font-weight: 700;
    line-height: 42px;
    letter-spacing: 0.05em;
    color: #ffffff;
    mix-blend-mode: normal;
    .m{
      margin-top: 100px;
    }
  .INSTAWEATHER {
    font-size: 36px;
    width: 301px;
    height: 42px;
  }
  .span{
    margin-left: auto;
    cursor:pointer;
    span{
      margin-left: 20px;
    }
      .f{
        background: rgba(255, 255, 255, 0.25);
        border-left:1px solid #000;
        padding: 15px;
        margin: 30px;
      }
  }
  ul{
    list-style: none;
  li{
    display: inline-block;
    margin-right: 20px;
    cursor: pointer;
  }
  }
  h6,h3{
    margin-top: 25px;
    font-size: 30px;
  }
  h6{
    font-size:20px;
  }
  h5{
    padding-top: 20px;
    font-size:35px;
  }
  h4{
    display:block;
    margin-top: 25px;
    font-size:30px;
    
  }
}
</style>
