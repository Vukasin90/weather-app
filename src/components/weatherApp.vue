<template> 
  <div class="weather-app" :class="[weather && weather.current.weather_descriptions[0].includes('Rain') ? 'weather-app--rain' : '', weather && weather.current.is_day === 'no' ? 'weather-app--night' : '', weather && weather.current.weather_descriptions[0].includes('Thunder') ? 'weather-app--rain' : '']">
	<div class="weather-app__overlay" :class="[weather ? 'weather-app__overlay--active' : '', weather && weather.current.weather_descriptions[0].includes('Rain') ? 'weather-app__overlay--rain' : '']"></div>
    <div class="weather-app__search-box">
      <input type="text" class="weather-app__search-bar" placeholder="Search..." v-model="query" @keypress="fetchWeather">
    </div>

    <div class="weather-app__wrapper" v-if="weather">
      <div class="weather-app__location-wrapper">
        <span class="weather-wrap__location">{{weather.location.name}}, {{weather.location.country}}</span>
        <span class="weather-app__date">{{ dateBuilder() }}</span>
      </div>

      <div class="weather-app__weather-box">
        <span class="weather-app__weather-temp"><img class="weather-app__weather-temp-icon" :src="weather.current.weather_icons[0]" alt="">{{weather.current.temperature}}&deg;</span>
        <span class="weather-app__weather-desc">{{weather.current.weather_descriptions[0]}}</span>
      </div>
    </div>
	<div class="weather-app__wrapper--welcome" v-else>Welcome, search your location</div>
  </div>
</template>

<script>
export default {
  name: 'weatherApp',
  data() {
    return {
		api_key: 'f476ea270ce79b2fdac0d0b3b441be9a',
		url_base: 'http://api.weatherstack.com/current',
		query: '',
		weather: null,
    }
  },

  methods: {
	fetchWeather(e) {
		const search = document.querySelector('.weather-app__search-bar');
		if(e.key === 'Enter') {
		fetch(`${this.url_base}?access_key=${this.api_key}&query=${this.query}&hourly=1`)
			.then(res => {
				return res.json();
			}).then(this.setResults).then(function() {
				search.value = '';
			});
		}
	},

    setResults (results) {
      this.weather = results;
      console.log(this.weather);
    },

    dateBuilder() {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "Spetember", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	.weather-app {
		transition: 0.4s;
		min-height: 100vh;
		padding: 25px 0 0;
		position: relative;
		background-image: url('../assets/cloud-blue-sky.jpg');
		background-size: cover;
		background-position: bottom;
	}

	.weather-app--rain {
		background-image: url('../assets/rain-clouds.jpg');
		transition: 0.4s;
	}

	.weather-app--night {
		background-image: url('../assets/nighttime.jpg');
		transition: 0.4s;
	}

	/* .weather-app--sunny {
		background-image: url('../assets/cloud-blue-sky.jpg');
	} */

	.weather-app__overlay {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.55));
		opacity: 0;
		transform: translateY(0);
		transition: all 1000ms ease-in-out
	}

	.weather-app__overlay--rain {
		background-image: url('../assets/raindrops.jpg');
		background-size: cover;
		background-repeat: no-repeat;
		background-position: center;
	}

	.weather-app__overlay--active {
		opacity: 1;
	}
	.weather-app__search-box {
		display: flex;
		justify-content: center;
		align-content: center;
		margin-bottom: 30px;
	}

	.weather-app__search-bar {
		display: block;
		width: 700px;
		padding: 15px;
		color: #313131;
		font-size: 20px;
		appearance: none;
		border:none;
		outline: none;
		background: none;
		box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
		background-color: rgba(255, 255, 255, 0.5);
		border-radius: 0px 16px 0px 16px;
		transition: 0.4s;
		position: relative;
		z-index: 2;
	}

	.weather-app__wrapper {
		position: relative;
		z-index: 2;
	}

	.weather-wrap__location {
		color: #FFF;
		font-size: 32px;
		font-weight: 500;
		text-align: center;
		text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
		display: block;
	}

	.weather-app__date {
		color: #FFF;
		font-size: 20px;
		font-weight: 300;
		font-style: italic;
		text-align: center;
	}

	.weather-app__weather-box {
		text-align: center;
		max-width: 730px;
		width: 100%;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		margin: 0 auto;
	}

	.weather-app__weather-temp {
		display: flex;
		align-items: center;
		justify-content: center;
		padding: 10px 25px;
		color: #FFF;
		font-size: 102px;
		font-weight: 900;
		text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
		background-color:rgba(255, 255, 255, 0.25);
		border-radius: 16px;
		margin: 30px 0px;
		box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
	}

	.weather-app__weather-temp-icon {
		margin-right: 20px;
		border-radius: 20px;
	}

	.weather-app__weather-desc {
		color: #FFF;
		font-size: 48px;
		font-weight: 700;
		font-style: italic;
		text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
	}

	.weather-app__wrapper--welcome {
		color: #FFF;
		font-size: 32px;
		font-weight: 500;
		text-align: center;
		text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
		display: block;
	}

</style>
