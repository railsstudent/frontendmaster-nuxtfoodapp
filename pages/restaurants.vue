<template>
  <main class="container restaurant">
    <div class="restaurantheading">
      <h1>Restaurants</h1>
      <AppSelect @change="selectedRestaurant = $event" />
    </div>
    <AppRestaurantInfo :datasource="filteredFoodData" />
  </main>
</template>

<script>
import AppRestaurantInfo from '@/components/AppRestaurantInfo';
import AppSelect from '@/components/AppSelect';
import { mapState } from 'vuex';

export default {
  components: {
    AppRestaurantInfo,
    AppSelect
  },
  computed: {
    ...mapState(['fooddata']),
    filteredFoodData() {
      if (this.selectedRestaurant) {
        const name = this.selectedRestaurant.toLowerCase()
        return this.fooddata.filter(item => item.name.toLowerCase().includes(name))
      }
      return this.fooddata
    }
  },
  data: function() {
    return {
      selectedRestaurant: ''
    }
  }
}
</script>

<style lang="scss" scoped></style>
