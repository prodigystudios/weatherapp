<template>
  <div class="wrapper">
    <input class="input" v-on:keydown.enter="fetchWeather()" v-model="query" type="text"
      placeholder="var vill du veta vädret?" />
    <div v-if="query.length > 1">
      <h1> <span style="text-transform:uppercase">{{ country }} {{ date }}</span></h1>
      <h1 class="temp">{{ temp }} °C</h1>
      <h1>{{ conditions }}</h1>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HomeView',
  data() {
    return {
      country: '',
      conditions: '',
      temp: '',
      query: '',
      date: ''
    }
  },
  methods: {
    fetchWeather() {
      fetch("https://weather.visualcrossing.com/VisualCrossingWebServices/rest/services/timeline/" + this.query + "?unitGroup=metric&include=current&key=HJ6JGEJRS4S4MZ7XBLDFDAZA2&contentType=json", {
        "method": "GET",
        "headers": {
        }
      })
        .then(response => response.json())
        .then(data => {
          this.country = data.address
          this.temp = data.currentConditions.temp
          this.conditions = data.currentConditions.conditions
          this.date = data.currentConditions.datetime
          console.log(data)
        })
        .catch(err => {
          console.error(err);
        });
    }
  }
}
</script>
<style scoped>
.input
{
  border-radius: 10px 0px 10px 0px;
  padding: 10px;
  background: rgba(0, 0, 0, 0.541);
  font-size: 16px;
  color: white;
  text-shadow: 2px 1.4px black;
}

h1
{
  color: black;
}

.wrapper
{
  padding: 30px;
  background: rgba(31, 18, 18, 0.342);
  border-radius: 15px;
  z-index: 100;
  position: absolute;
  top: 30%;
  left: 45%;
}

.temp
{
  display: flex;
  justify-content: center;
  align-items: center;
  border: 2px solid gray;
  box-shadow: 3px 3px 3px 50px inset rgba(0, 0, 0, 0.541);
  border-radius: 10px;
  width: 100%;
  height: 100px;
  color: white;
  font-size: 40px;
}

@media (max-width: 425px)
{
  .wrapper
  {
      display:flex;
  flex-direction: column;
  gap: 100px;
    left: 57%;
    height: 60%;
    width: 100%;
  }
}
</style>