<template>
	<div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
		<main>

			<div class="search-box">
				<input type="text" class="search-bar" placeholder="Search..." v-model="query" @keypress="fetchWeather">
			</div>

			<div class="weather-wrap" v-if="typeof weather.main != 'undefined' ">
				<div class="location-box">
					<div class="location">{{weather.name}}, {{weather.sys.country}}</div>
					<div class="date">{{getDate()}}</div>
				</div>
				<div class="weather-box">
					<div class="temp">{{ Math.round(weather.main.temp)}} &deg;C</div>
					<div class="weather">{{weather.weather[0].main}}</div>
				</div>
			</div>
		</main>
	</div>
</template>

<script>

export default {
  name: 'App',
	data() {
		return {
			api_key: '0bc3e848ccf3ea1c689959dec75a7c96',
			url_base: 'https://api.openweathermap.org/data/2.5/',
			query: '',
			weather: {}
		}
	},
	methods: {
		fetchWeather(e) {
			if (e.key == 'Enter') {
				fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
					.then(res => res.json()).then(this.setResults);
				this.query = '';
			}
		},
		setResults(results) {
			this.weather = results;
			console.log(this.weather)
		},
		getDate() {
			let d = new Date();
			let days = ['Sunday','Monday','Tuesday','Wednesday','Thursday','Friday','Saturday'];
			let months = ['January','February', 'March', 'April', 'May', 'June', 'July','August', 'September','October','November','December'];

			let day = days[d.getDay()];
			let date = d.getDate();
			let month = months[d.getMonth()];
			let year = d.getFullYear();
			return `${day} ${date} ${month} ${year}`;
		}
	},
}
</script>

<style>
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
}
body {
	font-family: 'montserrat', sans-serif;
}
#app {
	background: url(./assets/cold-bg.jpg) center bottom/cover;
	transition: .4s;
}
#app.warm {
	background: url(./assets/warm-bg.jpg) center bottom/cover;
}
main {
	min-height: 100vh;
	padding: 25px;
	background: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
}
.search-box {
	width: 100%;
	margin-bottom: 30px;
}
.search-bar {
	width: 100%;
	padding: 15px;
	color:#313131;
	font-size: 20px;
	border:none;
	outline: none;
	appearance: none;
	background: rgba(255,255,255,0.5);
	box-shadow: 0 0 8px rgba(0,0,0,0.25);
	border-radius: 0 16px 0 16px;
	transition: .4s;
}
.search-bar:focus {
	background: rgba(255, 255, 255, 0.75);
	box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
	border-radius: 16px 0 16px 0;
}
.location {
	color:#fff;
	font-size: 32px;
	font-weight: 500;
	text-align: center;
	text-shadow: 1px 3px rgba(0,0,0,0.25)
}
.date {
	color: #fff;
	font-size: 20px;
	font-weight: 300;
	font-style: italic;
	text-align: center;
}
.weather-box {
	text-align: center;
}
.temp {
	display: inline-block;
	padding: 10px 25px;
	color:#fff;
	font-size: 100px;
	font-weight: 900;
	text-shadow: 3px 6px rgba(0,0,0,0.25);
	background: rgba(255,255,255,0.25);
	border-radius: 16px;
	margin: 30px 0;
	box-shadow: 3px 6px rgba(0,0,0,0.25);
}
.weather {
	color:#fff;
	font-size: 48px;
	font-weight: 700;
	font-style: italic;
	text-shadow: 3px 6px rgba(0,0,0,0.25);
}
</style>
