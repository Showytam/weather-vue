<template>
  <div id="main" :class="isDay ? 'day' : 'night'">
    <div class="container">
      <h1 class="title">Weather</h1>

      <form class="search" @submit.prevent="getWeather">
        <div class="castom-input">
          <input
            type="text"
            class="search-city"
            v-model="citySearch"
            placeholder="Enter location"
            autocomplete="off"
          />
          <img src="../assets/image/search.svg" alt="search" @click="getWeather">
        </div>
      </form>
      <p class="found" v-if="cityFound">No city found</p>

      <div class="body" v-if="visible">
        <div class="card-top">
          <div class="city-name">
            <h2>{{ weather.cityName }} {{ weather.country }}</h2>
            <div :class="isDay ? 'dateDay' : 'dateNight'">
              {{ dateBuilder() }}
            </div>
          </div>
        </div>

        <div class="temperature">
          <p>{{ weather.temperature }}°C</p>
        </div>

        <div :class="isDay ? 'descriptionDay' : 'descriptionNight'">
          <p>{{ weather.description }}</p>
        </div>

        <div class="row">
          <div :class="isDay ? 'mainDay' : 'mainNight'">
            <p>{{ weather.minTemp }}°c</p>
            <p class="option">Min</p>
          </div>
          <div :class="isDay ? 'mainDay' : 'mainNight'">
            <p>{{ weather.maxTemp }}°c</p>
            <p class="option">Max</p>
          </div>
          <div :class="isDay ? 'mainDay' : 'mainNight'">
            <p>{{ weather.feelsLike }}°c</p>
            <p class="option">Feels Like</p>
          </div>
          <div :class="isDay ? 'mainDay' : 'mainNight'">
            <p>{{ weather.speed }}m/s</p>
            <p class="option">Speed</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'useWeather',
  data() {
    return {
      visible: false,
      cityFound: false,
      isDay: true,
      citySearch: '',

      weather: {
        cityName: 'City',
        country: '',
        temperature: '-',
        description: '...',
        minTemp: '-',
        maxTemp: '-',
        feelsLike: '-',
        speed: '-',
      },
    }
  },
  methods: {
    getWeather: async function () {
      const key = '1dd0076b8a67f62edc6067013358263b'
      const baseURL = `https://api.openweathermap.org/data/2.5/weather?q=${this.citySearch}&appid=${key}&units=metric`

      try {
        const response = await fetch(baseURL)
        const data = await response.json()
        console.log(data)
        this.citySearch = ''
        this.weather.cityName = data.name
        this.weather.country = data.sys.country
        this.weather.temperature = Math.round(data.main.temp)
        this.weather.description = data.weather[0].description
        this.weather.minTemp = Math.round(data.main.temp_min)
        this.weather.maxTemp = Math.round(data.main.temp_max)
        this.weather.feelsLike = Math.round(data.main.feels_like)
        this.weather.speed = data.wind.speed

        const timeOfDay = data.weather[0].icon

        if (timeOfDay.includes('n')) {
          this.isDay = false
        } else {
          this.isDay = true
        }

        this.visible = true
        this.cityFound = false
      } catch (error) {
        console.log(error)
        this.cityFound = true
        this.visible = false
      }
    },

    dateBuilder() {
      let d = new Date()
      let months = [
        'January',
        'February',
        'March',
        'April',
        'May',
        'June',
        'July',
        'August',
        'September',
        'October',
        'November',
        'December',
      ]
      let days = [
        'Sunday',
        'Monday',
        'Tuesday',
        'Wednesday',
        'Thursday',
        'Friday',
        'Saturday',
      ]
      let day = days[d.getDay()]
      let date = d.getDate()
      let month = months[d.getMonth()]
      let year = d.getFullYear()
      return `${day} ${date} ${month} ${year}`
    },
  },
}
</script>

<style scoped>
#main {
  height: 100vh;
  width: 100vw;
}

.day {
  background-image: url(../assets/image/bg14.jpg);
  background-size: cover;
}

.night {
  background-image: url(../assets/image/bg10.jpg);
  background-size: cover;
}

.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.title {
  margin-top: 30px;
  margin-bottom: 20px;
  color: #fff;
}

.search {
  margin-bottom: 20px;
}

.castom-input {
  position: relative;
}

.castom-input img {
  position: absolute;
  top: 0;
  bottom: 0;
  right: 15px;
  margin: auto 0;
  width: 25px;
  height: 25px;
  cursor: pointer;
}

.search-city {
  display: block;
  max-width: 400px;
  min-width: 320px;
  padding: 15px;

  color: #4e4d4d;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.8);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.found {
  font-size: 22px;
  color: #fff;
  margin-top: 30px;
}

.body {
  max-width: 400px;
  min-width: 320px;
  max-height: 400px;
  min-height: 340px;
  background: rgba(255, 255, 255, 0.3);
  backdrop-filter: blur(5px);
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
  color: #fff;
  box-shadow: 0 25px 45px rgb(0, 0, 0, 0.1);
  border-radius: 0px 16px 0px 16px;
}

.city-name {
  font-size: 20px;
  margin-top: 15px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.dateDay {
  font-size: 18px;
  color: rgb(69, 79, 89);
}


.temperature {
  display: block;
  margin-top: 40px;
}

.temperature p {
  font-size: 50px;
}

.descriptionDay p {
  font-size: 18px;
  color: rgb(69, 79, 89);
}


.row {
  display: flex;
  margin-top: 50px;
  background: rgba(255, 255, 255, 0.3);
  border-radius: 10px;
}

.row div {
  padding: 10px 10px;
}

.mainDay {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  font-size: 14px;
  color: rgb(69, 79, 89);
}

.mainNight {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  font-size: 14px;
  color: #fff;
}

.option,
.descriptionNight,
.dateNight {
  font-size: 18px;
  color: #fff;
}
</style>
