<template>
  <div>
    <form @submit.prevent="makeApiCall">
      <input class="searchinput" placeholder="City" v-model="cityName" />
      <button class="submitbutton" type="submit">Search</button>
    </form>
    <div v-if="loading" class="loader"></div>
    <div v-if="weather.main && !loading" class="show">
      <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
      <div class="maininfo">
        {{ Math.round(weather.main.temp) }} °C
        {{ weather.weather[0].main }}
      </div>
    </div>
    <div v-else-if="!loading" class="nodata">
      ENTER CITY NAME TO SHOW RESULTS
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      cityName: "",
      weather: {},
      loading: false,
      weatherImg: "clear",
    };
  },
  methods: {
    makeApiCall() {
      if (!this.cityName) return;
      this.loading = true;
      fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${this.cityName}&appid=0cfb40e84cb92fb4ce1d834ef8f8c260&units=metric`
      )
        .then((data) => {
          return data.json();
        })
        .then((d) => {
          this.weather = d;
          document.title = `Weather for ${this.weather.name}`;
          this.loading = false;
          if (Math.round(d.main.temp) < 5) {
            return (document.body.classList = "cold");
          }
          document.body.classList = d.weather[0].main.toLowerCase();
        });
    },
  },
};
</script>

<style>
body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen,
    Ubuntu, Cantarell, "Open Sans", "Helvetica Neue", sans-serif;
  background-repeat: no-repeat;
  background-image: url("./assets/clear.jpg");
  transition: 4s fade;
}
.clear {
  background-image: url("./assets/clear.jpg");
}
.cold {
  background-image: url("./assets/cold.jpg");
}
.rain {
  background-image: url("./assets/rain.jpg");
}
.clouds {
  background-image: url("./assets/clouds.jpg");
}
.searchinput {
  margin-top: 0.5rem;
  align-content: center;
  align-items: center;
  text-align: center;
  font-size: 20px;
  border: none;
  height: 30px;
  border-radius: 200px;
  background-color: rgba(255, 255, 255, 0.6);
  width: 100%;
  transition-delay: 0ms;
  transition: 3s ease;
}
.searchinput:focus {
  outline: none;
  border-radius: 10px;
}

.submitbutton {
  display: none;
}
.show {
  display: flexbox;
  margin-top: 1rem;
  background: none;
}
.location {
  color: whitesmoke;
  text-align: center;
  font-size: 2rem;
  text-shadow: 2px 4px 7px black;
}
.nodata {
  color: whitesmoke;
  text-align: center;
  font-size: 6rem;
  text-shadow: 2px 4px 7px black;
}
.maininfo {
  color: whitesmoke;
  text-align: center;
  text-shadow: 2px 4px 7px black;
  font-size: 4rem;
}
.loader {
  align-content: center;
  align-items: center;
  text-align: center;
  margin: 6rem 45vw;
  border: 16px solid #f3f3f3; /* Light grey */
  border-top: 16px solid #3498db; /* Blue */
  border-radius: 50%;
  width: 120px;
  height: 120px;
  animation: spin 2s linear infinite;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
@media screen and (min-width: 670px) {
  body {
    background-size: 100vw 100vh;
  }
  .location {
    font-size: 5rem;
    text-shadow: 2px 4px 7px black;
  }
  .maininfo {
    font-size: 6rem;
  }
}
@media screen and (min-width: 1000px) {
  .searchinput {
    width: 30vw;
  }
  .maininfo {
    font-size: 8rem;
  }
}
@media screen and (min-height: 950px) {
  body {
    background-size: 100vw 100vh;
  }
}
</style>
