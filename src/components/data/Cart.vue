<script>
import currency from "currency.js";

export default {
  computed: {
    cart() {
      return this.$store.state.cart;
    },

    cartTotal() {
      const total = this.cart.reduce(
        (total, item) =>
          total.add(currency(item.price.amount).multiply(item.qty)),
        currency(0, { formatWithSymbol: true, symbol: "£" })
      );
      return total.format();
    }
  },

  render() {
    return this.$scopedSlots.default({
      cart: this.cart,
      cartTotal: this.cartTotal
    });
  }
};
</script>