<template>
  <nav>
    <ul>
      <li><nuxt-link exact to="/"><AppLogo></AppLogo></nuxt-link></li>
      <li><nuxt-link to="/restaurants">Restaurants</nuxt-link></li>
    </ul>
    <div v-if="count > 0" class="smallnum">{{ count }}</div>
    <nuxt-link to="/cart">Cart</nuxt-link>
  </nav>
</template>

<script>
import AppLogo from '@/components/AppLogo.vue';
import { mapGetters } from 'vuex';

export default {
  components: {
    AppLogo
  },
  computed: {
    ...mapGetters(['count'])
  }
}
</script>

<style lang="scss" scoped></style>
