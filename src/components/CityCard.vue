<template>
  <div class="pb-4 city-card">
    <h2 class="city-card__city-name">Weather in {{ city }}, UA</h2>
    <div class="city-card__forecast">
      <div class="city-card__forecast-img">
        <img :src="`http://openweathermap.org/img/wn/${this.formattedForecastData[0].weatherIcon}@2x.png`" alt="">
      </div>
      <div class="city-card__temperature">{{ this.formattedForecastData[0].temperature }}°C</div>
    </div>
    <div class="pl-3 city-card__overcast">
      {{ this.formattedForecastData[0].overcast }}
    </div>
    <div class="mb-4 pl-3 city-card__date">
      {{ this.formattedForecastData[0].date }}
    </div>
    <div class="city-card_group-buttons">
      <button class="city-card__btn">
        Details
      </button>
      <button
        class="city-card__btn"
        @click="getForecastData"
      >
        Refresh
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CityCard',

  props: {
    city: String,
  },

  watch: {
    forecastData() {
      this.getFormattedForecastData();
    },
  },

  data: () => ({
    forecastData: [],
    formattedForecastData: [],
  }),

  mounted() {
    this.getForecastData();
  },

  methods: {
    getForecastData() {
      this.$http.get(`http://api.openweathermap.org/data/2.5/forecast?q=${this.city}&units=metric&appid=b73f83fed9c3ed5c0c4b2ef9c0782101`)
        .then((response) => {
          this.forecastData = response.data.list;
        });
    },

    getFormattedForecastData() {
      this.formattedForecastData = this.forecastData.map(item => ({
        temperature: Math.round(item.main.temp),
        overcast: item.weather[0].description,
        weatherIcon: item.weather[0].icon,
        date: this.getFormettedDate(item.dt),
      }));
    },

    getFormettedDate(val) {
      const time = String(new Date(val * 1000)).slice(16, 21);
      const month = String(new Date(val * 1000)).slice(4, 7);
      const day = String(new Date(val * 1000)).slice(8, 10);

      return `${time} ${month} ${day}`;
    },
  },
};
</script>

<style scoped lang="scss">
@import '@/design/index';

.city-card {
  border: 1px solid $blue-grey;
  border-radius: 5px;
  box-shadow: 0px 0px 10px 1px rgba(0,0,0,0.2);
  width: 300px;

  &__forecast {
    display: flex;
  }

  &__temperature {
    font-size: 42px;
    font-weight: 700;
    line-height: 100px;
  }

  &__overcast {
    font-weight: 700;
  }

  &__date {
    font-size: 15px;
    font-weight: 600;
  }

  &_group-buttons {
    display: flex;
    justify-content: space-around;
  }

  &__btn {
    font-size: 16px;
  }
}
</style>
