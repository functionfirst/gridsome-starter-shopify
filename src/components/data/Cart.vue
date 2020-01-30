<script>
export default {
  computed: {
    cart() {
      return this.$store.state.cart;
    }
  },

  render() {
    return this.$scopedSlots.default({
      cart: this.cart
    });
  }
};
</script>