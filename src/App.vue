<template>
	<div id="app" :class="typeof weather.main != 'undefined'
		&& weather.main.temp > 16 ? 'warm' : ''
	">
		<main>
			<div class="search-box">
				<input type="text" class="search-bar" placeholder="Search for a place or city..." v-model="query"
					@keypress="fetchWeather" />
			</div>

			<div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
				<div class="location-box">
					<div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
					<div class="date">{{ dateFormat() }}</div>
				</div>

				<div class="weather-box">
					<div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
					<div class="weather">{{ weather.weather[0].main }}</div>
				</div>
			</div>
		</main>
	</div>
</template>

<script>
	export default {
		name: 'app',
		data() {
			return {
				api_key: 'cb1be4eda75c3ec64fc78ab8c6555012',
				url_base: 'https://api.openweathermap.org/data/2.5/',
				query: '',
				weather: {}
			}
		},

		methods: {
			fetchWeather(e) {
				if (e.key == "Enter") {
					fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
						.then(res => {
							return res.json();
						}).then(this.setResults);
				}
			},
			setResults(results) {
				this.weather = results;
			},
			dateFormat() {
				let d = new Date();
				let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September",
					"October", "November", "December"
				];
				let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

				let day = days[d.getDay()];
				let date = d.getDate();
				let month = months[d.getMonth()];
				let year = d.getFullYear();

				return `${day}, ${date} ${month} ${year}`;
			}
		}
	}
</script>

<style>
	* {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}

	body {
		font-family: 'IBM Plex Mono', monospace;
	}

	#app {
		background-image: url('./assets/cold-bg.jpg');
		background-size: cover;
		background-position: bottom;
		transition: 0.4s;
	}

	#app.warm {
		background-image: url('./assets/warm-bg.jpg');
	}

	main {
		min-height: 100vh;
		padding: 4rem;

		background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
	}

	.search-box {
		width: 100%;
		margin-bottom: 3rem;
	}

	.search-box .search-bar {
		display: block;
		width: 100%;
		padding: 1.6rem;

		color: #313131;
		font-size: 1.6rem;

		appearance: none;
		border: none;
		outline: none;
		background: none;

		box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
		background-color: rgba(255, 255, 255, 0.5);
		border-radius: 1rem;
		transition: 0.4s;
	}

	.search-box .search-bar:focus {
		box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
		background-color: rgba(255, 255, 255, 0.75);
		border-radius: 3rem;
	}

	.location-box .location {
		color: #FFF;
		font-size: 2rem;
		font-weight: 500;
		text-align: center;
	}

	.location-box .date {
		color: #FFF;
		font-size: 1.6rem;
		font-weight: 300;
		text-align: center;
		margin-top: 0.5rem;
	}

	.weather-box {
		text-align: center;
	}

	.weather-box .temp {
		display: inline-block;
		padding: 2rem;
		color: #FFF;
		font-size: 6rem;
		font-weight: 800;

		background-color: rgba(255, 255, 255, 0.35);
		border-radius: 2rem;
		margin: 1rem;

	}

	.weather-box .weather {
		color: #FFF;
		font-size: 3rem;
		font-weight: 500;
	}
</style>
