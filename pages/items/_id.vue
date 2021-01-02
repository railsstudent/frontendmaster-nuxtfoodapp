<template>
  <main class="container">
    <!-- <div class="container dynamic">
      <pre>
        id: {{ id }}
        currentItem: {{ currentItem }}
        quantity {{ quantity }}
      </pre>
    </div> -->
    <section class="image">
      <div :style="`background: url(../${currentItem.img}) no-repeat center center`"></div>
    </section>
    <section class="details">
      <h1>{{ currentItem.item }}</h1>
      <h3>{{ priceFormatting(currentItem.price) }}</h3>
      <div class="quantity">
        <input type="number" v-model="quantity" />
        <button class="primary">Add to cart - {{ priceFormatting(total) }}</button>
      </div>
      <fieldset v-if="currentItem.options">
        <legend>Options</legend>
        <div v-for="option of currentItem.options" :key="option">
          <input type="radio" :id="option" name="option" :value="option">
          <label :for="option">{{option}}</label>
        </div>
      </fieldset>
      <fieldset v-if="currentItem.addOns">
        <legend>Add Ons</legend>
        <div v-for="addOn of currentItem.addOns" :key="addOn">
          <input type="checkbox" :id="addOn" name="addOn" :value="addOn">
          <label :for="addOn">{{addOn}}</label>
        </div>
      </fieldset>
    </section>
    <section class="options">
      <h3>Description</h3>
      <p>{{ currentItem.description }}</p>
    </section>
  </main>
</template>

<script>
import { mapState } from 'vuex';

export default {
  data() {
    return {
      id: this.$route.params.id,
      quantity: 1
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
    },
    total() {
      return this.quantity * this.currentItem.price
    }
  },
  methods: {
    priceFormatting(price) {
      return `$ ${price.toFixed(2)}`
    }
  }
}
</script>

<style lang="scss" scoped>
  main.container {
    width: 1000px;
    margin: 100px auto;
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 1fr 1fr 1fr;
    grid-column-gap: 30px;
    grid-row-gap: 30px;
  }

  .image {
    grid-column: 1 / 2;
    grid-row: 1 / 3;

    div {
      height: 100%;
    }
  }

  .details {
    grid-column: 2 / 3;
    grid-row: 1 / 4;

    h1, h3 {
      margin-bottom: 1rem;
    }
  }

  .options {
    grid-column: 1 / 2;
    grid-row: 3 / 4;
  }

  input[type="checkbox"], input[type="radio"] {
    margin-top: 1rem;
  }
</style>
