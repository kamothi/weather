<template>  
<div id = "app"  >
  <h1>오늘의 날씨는 뭘까요?</h1>
  <div>
    <input type ="text" v-model = "city" placeholder="search.." @keyup.enter="updateWeather" />
  </div>
  <div v-if = "information==''"></div> 
  <div id="w" v-else v-bind:class="Math.round((main.temp/10 -32)*5/9) >= 0 ? 'warm' : 'cold'">
    <div id="location">
      {{information.name}},{{sys.country}}
    </div>
    <div id="date">
      {{whatDate()}}
    </div>
    <div id="temp">
        {{Math.round((main.temp/10 -32)*5/9)}}
    </div>
    <div id= "wind">
      바람세기: {{wind.speed}}
    </div>
    <div id="cloud">
      구름의 양: {{information.clouds.all}}%
    </div>
  </div>
</div>
</template>

<script>
  export default{
    data : function(){
      return {
        api_key : "9944b131b16fa52eb070ac12771b7eb4",
        url : "https://api.openweathermap.org/data/2.5/",
        city : "",
        information:"",
        weather:"",
        wind: "",
        main:"",
        sys:"",
      }
    },
    methods: {
      updateWeather: function(){
        let fetchurl = "https://api.openweathermap.org/data/2.5/weather?q="+this.city +"&appid=9944b131b16fa52eb070ac12771b7eb4";
        fetch(fetchurl)
        .then((res) =>{
          console.log(res);
          return res.json();
        })
        .then ((results) => {
          return this.setResult(results);
        });
      },
      setResult: function(results){
        this.information = results;
        this.weather= this.information.weather;
        this.wind =this.information.wind;
        this.main=this.information.main;
        this.sys = this.information.sys;
      },
      whatDate: function(){
        let d = new Date();
        let months = [
          "January",
          "February",
          "March",
          "April",
          "May",
          "June",
          "July",
          "August",
          "September",
          "October",
          "November",
          "December",
        ];
        let days = [
          "Sunday",
          "Monday",
          "Tuesday",
          "Wednesday",
          "Thursday",
          "Friday",
          "Saturday",
        ];
        let day = days[d.getDay()];
        let date = d.getDate();
        let month = months[d.getMonth()];
        let year = d.getFullYear();
        return `${day} ${date} ${month} ${year}`;
      },
    }
  };
</script>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width: 50%;
  margin: auto;
}

.warm{
  background-color: yellowgreen;
}
.cold{
  background-color: black;
}
input {
  width: 50%;
  border: 1px solid #bbb;
  border-radius: 8px;
}
#w{
  color: white;
  width: 70%;
  margin: auto;
  border: 1px solid #bbb;
  border-radius: 8px;
}
#temp{
  font-size: 40px;
}
</style>