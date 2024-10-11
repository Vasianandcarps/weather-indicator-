<script>

import axios from 'axios'
export default {
  data() {
    return {
      city: "",
      error: "",
      info:null,
      
    };
  },
  mounted() {
    this.setInitialBg();
  },
  computed:{
    cityName(){
      return this.city
    },
    showTemp(){
      return "Temperature:" + this.info.main.temp
    },
    showFeelsLike(){
      return "Feels like:" + this.info.main.feels_like
    },
    showMinTemp(){
      return "Mini temperature:" +this.info.main.temp_min
    },
    showMaxTemp(){
      return "Maximal temperature:" +this.info.main.temp_max
    },
  },

  methods:{
   
    getWeather(){
     if(this.city.trim().length<2){
      this.error = "Input more then 1 symbols"
      return false;
     }
     this.error="";
     axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=4e6578e08fb847eaa692f6b10dc91179`)
    .then(res => {
        this.info = res.data;

      
        this.termoBg(this.info.main.feels_like);
      })
      .catch(err => {
        this.error = "Failed to fetch weather data";
        this.info = null;
      });
    },
    termoBg(temp) {
      if (typeof temp !== 'number') return; // Перевірка, чи temp — число
      if (temp < 10) {
        document.body.style.background = "linear-gradient(135deg, rgba(2,0,36,1) 0%, rgba(9,9,121,1) 35%, rgba(0,212,255,1) 100%)";
      } else if (temp > 25) {
        document.body.style.background = " linear-gradient(45deg, rgba(253,29,29,1) 0%, rgba(252,176,69,1) 100%)";
      } else {
        document.body.style.background = "linear-gradient(0deg, rgba(34,193,195,1) 0%, rgba(253,187,45,1) 100%)";
      }
    }
  },
    mounted() {
   
    this.termoBg(20);
  }
  
};
</script>

<template>
  <div class="wrapper">
    <h1>Weather app</h1>
    <p>Weather in {{ city=="" ? " your city" : cityName }}</p>
    <input type="text" v-model="city" placeholder="Input city" />
    <button v-if="city !=''" @click="getWeather() ">Search</button>
    <button disabled v-else>Input city</button>
    <p class="error">{{ error }}</p>
    <div v-if="info !=null">
      <p>{{ showTemp }}</p>
      <p>{{ showFeelsLike }}</p>
      <p>{{ showMinTemp }}</p>
      <p>{{ showMaxTemp }}</p>
    </div>
  </div>
</template>

<style scoped>
.error{
  color:#d03939;
}
.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  background: #1d0f0f;
  color: white;
  padding: 20px;
  text-align: center;
}
.wrapper h1 {
  margin-top: 50px;
}

.wrapper p {
  margin-top: 20px;
}

.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #7b7c81;
  color: white;
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: #d4dbe2;
}

.wrapper button {
  background:   #e49f0a;
  color: white;
  border-radius: 10px;
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  transition: transform 500ms ease;
}

.wrapper button:disabled {
  background: #706d67;
  border: 0px;
  cursor: not-allowed;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(5px);
}
</style>
