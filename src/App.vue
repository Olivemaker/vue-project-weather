<script>
import axios from 'axios';

export default {
 data(){
  return{
    city: "",
    error: "",
    info: null,
  }
 },
 computed:{
  cityName(){
    return "городе "+ this.city
  },
  temp(){
    if(this.info.main.temp < 0){
      return "-" + this.info.main.temp
    }else if(this.info.main.temp > 0){
      return "+" + this.info.main.temp
    }
    return this.info.main.temp
  },
  feelsLike(){
    return "Ощущается как:" + this.info.main.feels_like
  },
  tempMin(){
    return "Минимальная температура:" + this.info.main.temp_min
  },
  tempMax(){
    return "Максимальная температура:" + this.info.main.temp_max
  },
  pressure(){
    return "Давление:" + this.info.main.pressure
  },
  humidity(){
    return "Влажность:" + this.info.main.humidity
  },

 },
 methods: {
  getWeather(){
    if(this.city.trim().length < 2){
      this.error = "Слишком короткое название"
      return false
    }
    this.error = ""
    axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=b6206d24745489d9275575e4a6b95a01`)
      .then(res => (this.info = res.data));
  }
 }
}
</script>


<template>
  <div className="app-container">
    <div className="Title">
      <h1>Погодное приложение</h1>
      <p>Узнай погоду в любом городе!</p>
    </div>

    <div className="form-city">
      <input type="text" v-model="city" placeholder="Введите город">
      <button v-if="city != ''" @click="getWeather()">Узнать погоду</button>
      <button disabled="" v-else>Узнать погоду</button>
    </div>

    <h1>Погода в {{  city == '' ? 'вашем городе' : cityName }}</h1>
    <p className="error">{{ error }}</p>

    <div className="weather" v-if="info != null">
      <h2>{{ temp }}</h2>
      <p style="grid-row-start: 2;">{{ feelsLike }}</p>
      <p>{{ tempMin }}</p>
      <p style="grid-row-start: 2;">{{ tempMax }}</p>
      <p>{{ pressure }}</p>
      <p>{{ humidity }}</p>
    </div>
  </div>
</template>



<style scoped>
h1{
  color: #140663;
}
.app-container{
  display: flex;
  background: linear-gradient(rgba(223, 220, 241, 0.6), rgba(223, 220, 241, 0.9)) ;
  width: 900px;
  height: 500px;
  border-radius: 20px;
  flex-direction: column;
  padding: 50px;
  gap: 20px;
}
.form-city{
  display: flex;
  width: 100%;
  height: 50px;
  gap: 50px;
}
input{
  width: 300px;
  padding: 0 10px;
  background-color: rgba(0, 0, 0, 0);
  border: none;
  outline: none;
  border-bottom: 2px solid rgb(31, 7, 141);
  font-size: 16px;
  transition: font-size 0.5s ease; 

}
input:hover, input:focus{
  font-size: 20px;
}
button{
  width: 200px;
  font-size: 16px;
  font-weight: 600;
  border-radius: 10px;
  border: none;
  background-color: rgb(59, 62, 192);
  color: #d1e9ff;
  transition: transform 0.2s ease-in-out;
  cursor: pointer;
}
button:hover{
  transform: translate(0, -5px);
}
button:disabled{
  background-color: rgb(90, 90, 90);
  transform: translate(0, 0);
  color: #c7c7c7;
  cursor: no-drop;  
}
.error{
  color: rgb(155, 0, 26);
  font-weight: 700;
}
.weather{
  display: grid;
  grid-template: repeat(2, 1fr) / repeat(3, 1fr);
  gap: 0 30px;
}
</style>