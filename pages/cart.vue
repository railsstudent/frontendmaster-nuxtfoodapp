<template>
  <main class="container cart">
    <h2>Cart</h2>
    <table>
      <thead>
        <tr>
          <th>Item</th>
          <th>Add Ons</th>
          <th>Amount</th>
          <th>Total Price</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item of cart" :key="item">
          <td>
            {{ item.name }}
            <span v-if="item.options">- {{item.options}}</span>
          </td>
          <td>
            <span v-for="addon of item.addOns" :key="addon" class="comma">
              {{ addon }}
            </span>
          </td>
          <td>{{ item.count }}</td>
          <td>${{ item.combinedPrice }}</td>
        </tr>
        <tr>
          <td colspan="3"></td>
          <td class="total">Total: ${{totalPrice}}</td>
        </tr>
      </tbody>
    </table>
  </main>
</template>

<script>
import { mapState, mapGetters } from 'vuex';

export default {
  computed: {
    ...mapState(['cart']),
    ...mapGetters(['totalPrice'])
  },
};
</script>

<style lang="scss" scoped></style>
