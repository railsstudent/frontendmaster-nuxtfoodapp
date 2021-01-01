<template>
  <div class="container dynamic">
    <pre>
      id: {{ id }}
      currentItem: {{ currentItem }}
    </pre>
  </div>
</template>

<script>
import { mapState } from 'vuex';

export default {
  data() {
    return {
      id: this.$route.params.id
    }
  },
  computed: {
    ...mapState(['fooddata']),
    currentItem() {
      for (const store of this.fooddata) {
        for (const menuItem of store.menu) {
          if (menuItem.id === this.id) {
            return menuItem
          }
        }
      }
      return null
    }
  },
}
</script>

<style lang="scss" scoped></style>
