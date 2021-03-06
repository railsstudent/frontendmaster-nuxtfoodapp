<template>
  <main class="container">
    <section class="image">
      <div :style="`background: url(../${currentItem.img}) no-repeat center center`"></div>
    </section>
    <section class="details">
      <h1 class="item">{{ currentItem.item }}</h1>
      <h3 class="price">{{ priceFormatting(currentItem.price) }}</h3>
      <div class="quantity">
        <input type="number" v-model="quantity" />
        <button class="primary" @click="addItemToCart">Add to cart - {{ priceFormatting(combinedPrice) }}</button>
      </div>
      <fieldset v-if="currentItem.options">
        <legend><h3>Options</h3></legend>
        <div v-for="option of currentItem.options" :key="option" >
          <input type="radio" :id="option" name="option" :value="option" v-model="$v.itemOptions.$model">
          <label :for="option">{{option}}</label>
        </div>
      </fieldset>
      <fieldset v-if="currentItem.addOns">
        <legend><h3>Add Ons</h3></legend>
        <div v-for="addOn of currentItem.addOns" :key="addOn">
          <input type="checkbox" :id="addOn" name="addOn" :value="addOn" v-model="$v.itemAddons.$model">
          <label :for="addOn">{{addOn}}</label>
        </div>
      </fieldset>
      <AppToast v-if="cartSubmitted">
        Order Submitted<br />
        Check out more <nuxt-link to="/restaurants">restaurants</nuxt-link>
      </AppToast>
      <AppToast v-else-if="error">
        Please select options and/or<br />
        add ons before continue
      </AppToast>
    </section>
    <section class="options">
      <h3>Description</h3>
      <p>{{ currentItem.description }}</p>
    </section>
  </main>
</template>

<script>
import { mapState, mapMutations } from 'vuex'
import AppToast from '@/components/AppToast'
import { required } from 'vuelidate/lib/validators'

export default {
  components: {
    AppToast
  },
  data() {
    return {
      id: this.$route.params.id,
      quantity: 1,
      itemOptions: '',
      itemAddons: [],
      itemSizeAndCost: [],
      cartSubmitted: false,
      error: false
    }
  },
  validations: {
    itemOptions: {
      required
    },
    itemAddons: {
      required
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
    combinedPrice() {
      const total = this.quantity * this.currentItem.price
      return total.toFixed(2)
    }
  },
  methods: {
    ...mapMutations(['addToCart']),
    priceFormatting(price) {
      return `$ ${price}`
    },
    addItemToCart() {
      const formData = {
        name: this.currentItem.item,
        count: this.quantity,
        options: this.itemOptions,
        addOns: this.itemAddons,
        combinedPrice: this.combinedPrice
      }
      const itemOptionsError = this.currentItem.options && !this.$v.itemOptions.required
      const itemAddonsError = this.currentItem.addOns && !this.$v.itemAddons.required

      this.cartSubmitted = false
      this.error = false
      if (itemOptionsError || itemAddonsError) {
        this.error = true
      } else {
        this.cartSubmitted = true
        this.addToCart(formData)
      }
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
    position: relative;

    h1.item, h3.price {
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
