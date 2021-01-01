<template>
  <main class="container restaurant">
    <div class="restaurantheading">
      <h1>Restaurants</h1>
      <AppSelect @change="selectedRestaurante = $event" />
      <pre>{{ $data }}</pre>
    </div>
    <AppRestaurantInfo />
  </main>
</template>

<script>
import AppRestaurantInfo from '@/components/AppRestaurantInfo';
import AppSelect from '@/components/AppSelect';

export default {
  components: {
    AppRestaurantInfo,
    AppSelect
  },
  data: function() {
    return {
      selectedRestaurante: ''
    }
  }
}
</script>

<style lang="scss" scoped></style>
