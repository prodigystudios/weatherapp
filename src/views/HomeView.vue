<template>

  <div class="container">
    <transition name="fade">
      <div class="wrapper">
        <input id="input-field" class="input" v-on:keydown.enter="fetchWeather()" v-model="query" type="text"
          placeholder="var vill du veta vädret?" />
        <transition name="fade">
          <div class="wheater-info-container" v-if="wheaterFetched">
            <h1> <span style="text-transform:uppercase">{{ wheaterData.address }} {{ currentTime }}</span></h1>
            <h1 class="temp">{{ wheaterData.currentConditions.temp }} °C</h1>
            <h1>{{ wheaterData.currentConditions.conditions }}</h1>
            <div class="days-container">
              <section class="days" v-for="(days, index) in wheaterData.days.slice(0, 7)" :key="index">
                <h3>{{ getDayOfWeek(days.datetime) }}</h3>
                <img class="icon" :src="getIcon(days.icon)">
                <h4>{{ days.temp }} °C</h4>
              </section>
            </div>
          </div>
          <div v-else-if="notFound">
            <h1>Staden du sökte efter finns ej, försök igen!</h1>
          </div>
        </transition>
      </div>
    </transition>
  </div>
  <animated-bg :key="updateview" :animatedBgToUse="currentWheaterStatus" />
</template>

<script>
import AnimatedBg from '@/components/AnimatedBg.vue';
export default {
  components: {
    AnimatedBg,
  },
  name: 'HomeView',
  data() {
    return {
      updateview: 0,
      wheaterData: [],
      currentWheaterStatus: '',
      currentTime: '',
      wheaterFetched: false,
      notFound: false,
      query: '',
      wheaterIcons: [
        {
          name: 'clear-day',
          icon: require('@/assets/Icons/Color/clear-day.png')
        },
        {
          name: 'clear-night',
          icon: require('@/assets/Icons/Color/clear-night.png')
        },
        {
          name: 'cloudy',
          icon: require('@/assets/Icons/Color/cloudy.png')
        },
        {
          name: 'fog',
          icon: require('@/assets/Icons/Color/fog.png')
        },
        {
          name: 'hail',
          icon: require('@/assets/Icons/Color/hail.png')
        },
        {
          name: 'partly-cloudy-day',
          icon: require('@/assets/Icons/Color/partly-cloudy-day.png')
        },
        {
          name: 'rain-snow-showers-day',
          icon: require('@/assets/Icons/Color/rain-snow-showers-day.png')
        },
        {
          name: 'rain-snow-showers-night',
          icon: require('@/assets/Icons/Color/rain-snow-showers-night.png')
        },
        {
          name: 'rain-snow',
          icon: require('@/assets/Icons/Color/rain-snow.png')
        },
        {
          name: 'rain',
          icon: require('@/assets/Icons/Color/rain.png')
        },
        {
          name: 'showers-day',
          icon: require('@/assets/Icons/Color/showers-day.png')
        },
        {
          name: 'showers-night',
          icon: require('@/assets/Icons/Color/showers-night.png')
        },
        {
          name: 'sleet',
          icon: require('@/assets/Icons/Color/sleet.png')
        },
        {
          name: 'snow-showers-day',
          icon: require('@/assets/Icons/Color/snow-showers-day.png')
        },
        {
          name: 'snow-showers-night',
          icon: require('@/assets/Icons/Color/snow-showers-night.png')
        },
        {
          name: 'snow',
          icon: require('@/assets/Icons/Color/snow.png')
        },
        {
          name: 'thunder-rain',
          icon: require('@/assets/Icons/Color/thunder-rain.png')
        },
        {
          name: 'thunder-showers-day',
          icon: require('@/assets/Icons/Color/thunder-showers-day.png')
        },
        {
          name: 'thunder-showers-night',
          icon: require('@/assets/Icons/Color/thunder-showers-night.png')
        },
        {
          icon: require('@/assets/Icons/Color/thunder.png')
        },
        {
          name: 'wind',
          icon: require('@/assets/Icons/Color/wind.png')
        },

      ],

      //geoLocation param
      longitude: '',
      latitude: '',

    }
  },
  methods: {
    fetchWeather() {
      fetch("https://weather.visualcrossing.com/VisualCrossingWebServices/rest/services/timeline/"
        + this.query +
        "?unitGroup=metric&include=current&key=HJ6JGEJRS4S4MZ7XBLDFDAZA2&contentType=json",
        {
          "method": "GET",
          "headers": {
          }
        })
        .then(response => response.json())
        .then(data => {
          this.wheaterData = data;
          this.notFound = false;
          this.wheaterFetched = true;
          this.currentTime = new Date().toLocaleTimeString();
          this.query = '';
          this.currentWheaterStatus = data.currentConditions.conditions;
          this.updateview++;
        })
        .catch(err => {
          this.notFound = true;
          this.wheaterFetched = false;
          this.query = '';
          console.error(err);
        });
    },
    getDayOfWeek(date) {
      const dayOfWeek = new Date(date).getDay();
      return isNaN(dayOfWeek) ? null :
        ['Söndag', 'Måndag', 'Tisdag', 'Onsdag', 'Torsdag', 'Fredag', 'Lördag'][dayOfWeek];
    },
    getIcon(name) {
      var singelIcon = '';
      this.wheaterIcons.forEach(element => {
        if (element.name == name) {
          singelIcon = element.icon;
        }
      });
      return singelIcon;
    },

    showPosition(position) {
      fetch('https://api.mapbox.com/geocoding/v5/mapbox.places/' 
      + position.coords.longitude + ',' + position.coords.latitude + 
      '.json?types=place%2Cpostcode%2Caddress&limit=1&access_token=pk.eyJ1IjoicHJvZGlneXN0dWRpbyIsImEiOiJjbDdqOTBxdHowYWpjM3hsanpqcnptcG5iIn0.XqDAMh6ptNOUAVND5z4j4w', {
        "method": "GET",
        "headers": {
        },
      })
      .then(response => response.json())
      .then(data => {
        this.query = data.features[0].context[1].text;
        this.fetchWeather();
      })
    }
  },
  created() {
    if(navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(this.showPosition)
      }else {
        console.log('cant get position');
      } 
  }
}
</script>
<style scoped>
.container
{
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 800px;
}

.wrapper
{
  display: flex;
  flex-direction: column;
  height: 525px;
  position: absolute;
  z-index: 100;
  transition: 2s;
}

.input
{
  border-radius: 10px 0px 10px 0px;
  padding: 10px;
  margin-bottom: 10px;
  background: rgba(0, 0, 0, 0.540);
  font-size: 16px;
  color: white;
  text-shadow: 2px 1.4px black;
  font-weight: bold;
  text-align: center;
  text-transform: uppercase;
}

.input::placeholder
{
  color: lightgray;
}

h1,
h3,
h4
{
  color: white;
}

.temp
{
  display: flex;
  justify-content: center;
  align-items: center;
  border: 2px solid white;
  box-shadow: 3px 3px 3px 50px inset rgba(0, 0, 0, 0.541);
  border-radius: 10px;
  width: 100%;
  height: 100px;
  color: white;
  font-size: 40px;
}

.wheater-info-container
{
  height: auto;
  background: rgba(31, 18, 18, 0.600);
  border: 2px solid white;
  border-radius: 15px;
  padding: 20px 30px;
}

.days-container
{
  width: 100%;
  display: flex;
  flex-direction: row;
  justify-content: center;
  column-gap: 20px;
  row-gap: 20px;
  flex-wrap: wrap;
  border-radius: 10px;
}

.days
{
  border: 2px solid white;
  border-radius: 8px;
  padding: 10px;
}

.icons
{
  width: 32px;
  height: 32px;
}

.fade-enter-active,
.fade-leave-active
{
  transition: opacity 1.5s ease;
}

.fade-enter-from,
.fade-leave-to
{
  opacity: 0;
}

@media (max-width: 425px)
{
  .days {
    width: 100%;
    font-size: 15px;
  }
}

@media (max-width: 650px) {
  .days {
    width: 100%;
  }
}
</style>