<template>
  <div class="home">
    <CityCard city="Kiev" />

  </div>
</template>

<script>
// @ is an alias to /src
import CityCard from '@/components/CityCard.vue';

export default {
  name: 'home',

  components: {
    CityCard,
  },
};
</script>
