<template>
<div id="app">
  <main>
    <div class="search-box">
      <input type="text"
             class="search-bar"
             placeholder="Search..."
             v-model="query"
             @keypress="fetchWeather"/>
    </div>
    <div class="weather-wrap" v-if="typeof weather.list != 'undefined'">
      <div class="location-box">
        <div class="location">{{ weather.city.name }}</div>
      </div>
      <div class="date mt-5">Today - {{ new Date().toDateString() }}</div>
      <div v-for="result in weather.list" :key="result.id">
        <div class="weather-box" v-if="new Date().toDateString() === new Date(result.dt_txt).toDateString()">
        <div class="weather">{{ new Date(result.dt_txt).getHours() }}h00</div>
        <div class="temp">{{ Math.round(result.main.temp)}}°C</div>
        <div class="weather">{{ result.weather[0].main }}</div>
      </div>
      </div>
      <div class="date mt-3">Tomorrow - {{this.tomorrow}}</div>
      <div v-for="result in weather.list" :key="result.id">
        <div class="weather-box" v-if="this.tomorrow === new Date(result.dt_txt).toDateString()">
          <div class="weather">{{ new Date(result.dt_txt).getHours() }}h00</div>
          <div class="temp">{{ Math.round(result.main.temp)}}°C</div>
          <div class="weather">{{ result.weather[0].main }}</div>
        </div>
      </div>
    </div>
  </main>
</div>
</template>

<script>

export default {
  name: 'app',
  data(){
    return{
      api_key: '74e2838c1bf60fb2dda4aefb0197928a',
      api_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      tomorrow: '',
    }
  },
  methods:{
    fetchWeather(e){
      if(e.key === "Enter"){
        fetch(`${this.api_base}forecast?q=${this.query}&appid=${this.api_key}&units=metric`)
        .then( res => res.json())
        .then(this.setResults)
        this.query = ''
        const date = new Date();
        date.setDate(date.getDate() + 1);
        return this.tomorrow = date.toDateString()
      }
    },
    setResults(results){
      this.weather = results
    },
  }
}
</script>
<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family: 'montserrat', sans-serif;
}
#app{
  background-image: url('./assets/background.png');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
main{
  min-height: 100vh;
  padding: 25px;


  /*background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));*/
}
@media (min-width:801px) {
  .weather-wrap {
    width: 80%;
    margin: auto;
  }
}
@media (min-width:801px) {
  .search-box {
    width: 50%;
    margin: auto;
    display: flex;
    justify-content: center;
  }

  .search-box .search-bar {
    display: block;
    width: 60%;
    padding: 15px;

    color: #313131;
    font-size: 20px;

    appearance: none;
    border: none;
    outline: none;
    background: none;

    box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 0px 16px 0px 16px;
    transition: 0.4s
  }
}
.search-box .search-bar:focus{
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}
.location-box .location{
  color: #fff;
  font-size: 32px;
  margin-top: 40px;
  margin-bottom: 40px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25)
}
.date{
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
  margin-top: 20px;
}
.weather-box{
  text-align: center;
  display:flex;
  justify-content: center;
}
.weather-box .temp{
  display: inline-block;
  padding:10px 25px;
  color: #fff;
  font-size: 22px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather{
  color: #fff;
  font-size: 18px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  margin: auto;
}
@media (min-width:320px)  {
  .search-box {
    width: 80%;
    margin: auto;
    display: flex;
    justify-content: center;
  }

  .search-box .search-bar {
    display: block;
    width: 80%;
    padding: 15px;

    color: #313131;
    font-size: 20px;

    appearance: none;
    border: none;
    outline: none;
    background: none;

    box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 0px 16px 0px 16px;
    transition: 0.4s
  }
}

</style>
