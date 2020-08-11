<template>
  <div v-bind:class="{warm : temperature > 23,cold: temperature < 23  }">
    <main>
      <img src="https://emojipedia-us.s3.dualstack.us-west-1.amazonaws.com/thumbs/160/openmoji/252/thermometer_1f321.png" alt="">
      <h1 style="margin-left: 10px;">Tigran Weather</h1>
      <div>
        <input v-model="query" type="text" class="search-bar" @keyup.enter="getWeather" placeholder="Search..."/>
      </div>
      <div v-if="weather">
        <div v-if="weather.main" class="weather-info">
          <iframe class="maps"
              width="30%"
              height="400"
              :src="`https://maps.google.com/maps?width=100%25&amp;height=600&amp;hl=en&amp;q=${weather.coord.lat},%20${weather.coord.lon}+(City)&amp;t=&amp;z=11&amp;ie=UTF8&amp;iwloc=B&amp;output=embed`">
          </iframe>
          <p class="name">{{weather.name}}, {{weather.sys.country}}</p>
            <p class="main-temp">{{Math.round(weather.main.temp)}}°C </p>
          <div class="min-max">
            <p class="max-temp">Max - {{Math.round(weather.main.temp_max)}}°C</p>
            <p class="min-temp">Min - {{Math.round(weather.main.temp_min)}}°C</p>
          </div>

          </div>
        <div v-else-if="weather.message">
          <h1 class="error-msg">{{weather.message}}</h1>
          <button class="clearButton" v-text="query==='' ? '😉' : 'Clear'" v-bind:disabled="query===''" @click="clearCity"></button>
        </div>
      </div>

    </main>
  </div>
</template>

<script>


export default {
  name: 'App',
  data () {
    return {
      api_key:'ef7ceb02677b4c58cf57f37fdf0a68f9',
      base_url: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      temperature: 14
    }
  },
  methods : {
    async getWeather(e) {
        const date = await fetch(`${this.base_url}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        const weather = await date.json()
        this.setResult(weather)
        this.query='';
    },
    setResult(weather) {
      this.weather = weather
      this.temperature = weather.main.temp
      console.log(weather)
    },
    clearCity() {
      this.query = ''
    },
    goHome() {
      location.reload()
    }
  }
}
</script>

<style lang="less">
* {
  margin: 0;
  padding: 0;
  box-sizing:border-box;
}
body {
  font-family: 'Ubuntu Mono', sans-serif
}
.maps {
  position: absolute;
  left:70px;
  top: 20%;
  border-radius: 20px;
  box-shadow:  0 0 5px 0 rgba(57,245,57,1);
  border: 2px solid rgba(57,245,57,1);
  outline: none;

}
.cold {
  position:fixed;
  width:100%;
  height:100%;
  top:0;
  left:0;
  z-index:1000;
  color:black;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='811' height='135.2' viewBox='0 0 600 100'%3E%3Cg stroke='%23FFF' stroke-width='0' stroke-miterlimit='10' %3E%3Ccircle fill='%23037B79' cx='0' cy='0' r='50'/%3E%3Ccircle fill='%2392DEBA' cx='100' cy='0' r='50'/%3E%3Ccircle fill='%23FFFFD8' cx='200' cy='0' r='50'/%3E%3Ccircle fill='%23CAF2FF' cx='300' cy='0' r='50'/%3E%3Ccircle fill='%236FCCFF' cx='400' cy='0' r='50'/%3E%3Ccircle fill='%23006EB4' cx='500' cy='0' r='50'/%3E%3Ccircle fill='%23037B79' cx='600' cy='0' r='50'/%3E%3Ccircle cx='-50' cy='50' r='50'/%3E%3Ccircle fill='%2353ac9a' cx='50' cy='50' r='50'/%3E%3Ccircle fill='%23ceefc1' cx='150' cy='50' r='50'/%3E%3Ccircle fill='%23ffffff' cx='250' cy='50' r='50'/%3E%3Ccircle fill='%239de0fe' cx='350' cy='50' r='50'/%3E%3Ccircle fill='%233e9cda' cx='450' cy='50' r='50'/%3E%3Ccircle fill='%2300789c' cx='550' cy='50' r='50'/%3E%3Ccircle cx='650' cy='50' r='50'/%3E%3Ccircle fill='%23037B79' cx='0' cy='100' r='50'/%3E%3Ccircle fill='%2392DEBA' cx='100' cy='100' r='50'/%3E%3Ccircle fill='%23FFFFD8' cx='200' cy='100' r='50'/%3E%3Ccircle fill='%23CAF2FF' cx='300' cy='100' r='50'/%3E%3Ccircle fill='%236FCCFF' cx='400' cy='100' r='50'/%3E%3Ccircle fill='%23006EB4' cx='500' cy='100' r='50'/%3E%3Ccircle fill='%23037B79' cx='600' cy='100' r='50'/%3E%3Ccircle cx='50' cy='150' r='50'/%3E%3Ccircle cx='150' cy='150' r='50'/%3E%3Ccircle cx='250' cy='150' r='50'/%3E%3Ccircle cx='350' cy='150' r='50'/%3E%3Ccircle cx='450' cy='150' r='50'/%3E%3Ccircle cx='550' cy='150' r='50'/%3E%3C/g%3E%3C/svg%3E");
  /*background-position: center;
  background-repeat: no-repeat;
  background-size: cover;*/
}
.warm {
  &:extend(.cold);
  color:white;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='840' height='140' viewBox='0 0 600 100'%3E%3Cg stroke='%23ff29a8' stroke-width='0' stroke-miterlimit='10' %3E%3Ccircle fill='%23037B79' cx='0' cy='0' r='50'/%3E%3Ccircle fill='%231425de' cx='100' cy='0' r='50'/%3E%3Ccircle fill='%23ff2ba0' cx='200' cy='0' r='50'/%3E%3Ccircle fill='%23ff4f0f' cx='300' cy='0' r='50'/%3E%3Ccircle fill='%23ffce1c' cx='400' cy='0' r='50'/%3E%3Ccircle fill='%23b40d0d' cx='500' cy='0' r='50'/%3E%3Ccircle fill='%23037B79' cx='600' cy='0' r='50'/%3E%3Ccircle cx='-50' cy='50' r='50'/%3E%3Ccircle fill='%23006cc6' cx='50' cy='50' r='50'/%3E%3Ccircle fill='%23bf00bf' cx='150' cy='50' r='50'/%3E%3Ccircle fill='%23ff6d1f' cx='250' cy='50' r='50'/%3E%3Ccircle fill='%23ff9400' cx='350' cy='50' r='50'/%3E%3Ccircle fill='%23e37700' cx='450' cy='50' r='50'/%3E%3Ccircle fill='%23536c01' cx='550' cy='50' r='50'/%3E%3Ccircle cx='650' cy='50' r='50'/%3E%3Ccircle fill='%23037B79' cx='0' cy='100' r='50'/%3E%3Ccircle fill='%231425de' cx='100' cy='100' r='50'/%3E%3Ccircle fill='%23ff2ba0' cx='200' cy='100' r='50'/%3E%3Ccircle fill='%23ff4f0f' cx='300' cy='100' r='50'/%3E%3Ccircle fill='%23ffce1c' cx='400' cy='100' r='50'/%3E%3Ccircle fill='%23b40d0d' cx='500' cy='100' r='50'/%3E%3Ccircle fill='%23037B79' cx='600' cy='100' r='50'/%3E%3Ccircle cx='50' cy='150' r='50'/%3E%3Ccircle cx='150' cy='150' r='50'/%3E%3Ccircle cx='250' cy='150' r='50'/%3E%3Ccircle cx='350' cy='150' r='50'/%3E%3Ccircle cx='450' cy='150' r='50'/%3E%3Ccircle cx='550' cy='150' r='50'/%3E%3C/g%3E%3C/svg%3E");
}
.name {
  font-family: "Courier 10 Pitch",sans-serif;
  font-size: 40px;
  margin-right:300px;
  font-weight: bold;

}
main {
  min-height:100%;
  min-width: 100%;
  margin-left:42%;
}
.main-temp {
  font-size: 100px;
  font-weight: bold;

  text-shadow: 2px 2px 0 #4074b5,
  2px -2px 0 #4074b5, -2px 2px 0 #4074b5,
  -2px -2px 0 #4074b5, 2px 0px 0 #4074b5,
  0px 2px 0 #4074b5, -2px 0px 0 #4074b5,
  0px -2px 0 #4074b5,
  0px 0px 10px rgba(51,51,51,0.69);
}
.search-bar {
  box-sizing: border-box;
  border: 2px solid #eee;
  padding: 6px 40px 6px 20px;
  border-radius: 5px;
  transition-duration: 0.3s;
  &:focus,&:after {
    box-shadow:  0 0 5px 0 rgba(57,57,245,1);
    border: 2px solid rgba(57,57,245,1);
    outline: none;
  }
}
img {
  margin-left: 35px;
}
.min-max {
  p {
    display: inline-block;
    font-weight: bold;
    font-size: 20px;
  }
  .min-temp {
    margin-left: 50px;
  }
}
.error-msg {
  position: absolute;
  top:33.5%;
  font-size: 100px;
  left:25%;
  font-family: "Ubuntu Mono", sans-serif;

}
.clearButton {
  margin-top: 100px;
  margin-left: 50px;
  padding: 10px 40px;
  border: 2px solid gray;
  border-radius: 5px;
  font-size: 20px;
  font-family: "Ubuntu Mono", sans-serif;
  color: white;
  background-color: #4074b5;
  &:focus,&:after {
    box-shadow:  0 0 5px 0 rgba(57,57,245,1);
    border: 2px solid rgba(57,57,245,1);
    outline: none;
  }
}
</style>










