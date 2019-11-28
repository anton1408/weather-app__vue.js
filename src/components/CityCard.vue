<template>
  <div class="city-card">
    <h2 class="city-card__city-name">Weather in {{ city }}, UA</h2>
    <div class="city-card__forecast">
      <div class="city-card__forecast-img">
        <img src="http://openweathermap.org/img/wn/04n@2x.png" alt="">
      </div>
      <div class="city-card__temperature">0°C</div>
    </div>
    <div class="pl-3 city-card__overcast">
      Overcast clouds
    </div>
    <div class="pl-3 city-card__date">
      17:58 Nov 27
    </div>

    <pre>{{ this.forecastData[0] }}</pre>
  </div>
</template>

<script lang="ts">
export default {
  name: 'CityCard',

  props: {
    city: String,
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
        temperature: item,
      }));
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import '@/design/index';

.city-card {
  border: 1px solid $blue-grey;
  border-radius: 5px;
  box-shadow: 0px 0px 10px 1px rgba(0,0,0,0.2);
  width: 300px;
  height: 400px;

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
}
</style>
