<template>
  <tr>
    <td>
      <router-link :to="item.product.get_absolute_url">{{
        item.product.name
      }}</router-link>
    </td>
    <td>₹{{ item.product.price }}</td>
    <td>{{ item.quantity }}</td>
    <a
      class="button is-danger is-small ml-5 mt-2 mb-2"
      @click="decrementQuantity(item)"
    >
      <span class="icon">
        <i class="fas fa-minus"></i>
      </span>
    </a>
    <a
      class="button is-success is-small mt-2 mb-2 ml-2"
      @click="incrementQuantity(item)"
    >
      <span class="icon">
        <i class="fas fa-plus"></i>
      </span>
    </a>
    <td>₹{{ getItemTotal(item).toFixed(2) }}</td>
    <td>
      <button class="delete is-large" @click="removeFromCart(item)"></button>
    </td>
  </tr>
</template>

<script>
export default {
  name: "CartItem",
  props: {
    initialItem: Object,
  },
  data() {
    return {
      item: this.initialItem,
    };
  },
  methods: {
    getItemTotal(item) {
      return item.quantity * item.product.price;
    },
    decrementQuantity(item) {
      item.quantity -= 1;
      if (item.quantity === 0) {
        item.quantity = 1;
      }
      this.updateCart();
    },
    incrementQuantity(item) {
      item.quantity += 1;
      this.updateCart();
    },
    updateCart() {
      localStorage.setItem("cart", JSON.stringify(this.$store.state.cart));
    },
    removeFromCart(item) {
      this.$emit("removeFromCart", item);
      this.updateCart();
    },
  },
};
</script>
