
<template>
  <div id="app" class="rain">
    <main>
      <div class="search__box">
        <input 
              type="text" 
              class="search__bar" 
              placeholder="Search...." 
              @keypress='fetchWeather'
              v-model="query"
              />
              
      </div>
      <div class="weather__wraper" v-if="typeof weather.name != 'undefined'">
        <div class="location__box">
          <div class="location">{{weather.name}} {{weather.sys.country}}</div>
          <div class="date">{{dateBuilder()}}</div>
        </div>
        <div v-if="weather.weather[0].main === 'Clouds'" class="video_wraper">
          <video class="video"  src="./assets/Clouds.mp4" loop autoplay muted></video>
        </div>  
        <div v-if="weather.weather[0].main === 'Clear'" class="video_wraper">
          <video class="video"  src="./assets/Clear.mp4" loop autoplay muted></video>
        </div>
        <div v-if="weather.weather[0].main === 'Rain'" class="video_wraper">
          <video class="video"  src="./assets/Drizzle.mp4" loop autoplay muted></video>
        </div>
        <div class="weather__box">
          <div class="temp">{{Math.round(weather.main.temp)}}°c</div>
          <div class="weather">{{weather.weather[0].main}}</div>
          <div class="wind"> Wind speed: {{weather.wind.speed}} m/s</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "8975af225d84606c6281c8b45039499c",
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query:'',
      weather: {},
      src:'',
    };
  },
  methods:{
  fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();  
          }).then(this.setResults);
      }
    },
    setResults(results){
      this.weather = results;
    },
    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "Montserrat", sans-serif;
}
#app {
  background-image: url("./assets/photo-1500964757637-c85e8a162699.avif");
  background-size: cover;
  background-position: bottom;
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}
.search__box {
  width: 100%;
  margin-bottom: 30px;
}
.search__box .search__bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  border-radius: 5px;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  transition: 0.4s;
}
.search__box .search__bar:hover {
  background-color: rgba(255, 255, 255, 0.7);
  box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
}
.weather__wraper{
  display: flex;
  flex-direction: column;
  align-items: center;
}
.location__box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.date {
  color: #fff;
  font-size: 20px;
  font-weight: 100;
  font-style: italic;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  margin: 20px;
}
.weather__box {
  text-align: center;
  position: absolute;
  top: 25%;
}
.weather__box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 100px;
  font-weight: 600;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 15px;
  margin: 30px 0;
  box-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.video_wraper{
  width: 600px;
}
.video{
  text-align: center;
  width: 100%;
  border-radius: 20px;
}
.weather__box .weather {
  color: #fff;
  font-size: 40px;
  font-weight: 600;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.wind{
  color: #fff;
  font-size: 18px;
  margin-top: 20px;
}
</style>
