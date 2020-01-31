<template>
  <DataCart v-slot="{ cart, cartTotal, removeItem }">
    <div class="sidebar" :class="sidebar ? '' : 'sidebar-offset'">
      <div>
        <div>
          Added Items
          ({{ cart.length }})
        </div>

        <button @click="close">&times;</button>
      </div>

      <div class="flex">
        <div v-for="product in cart" :key="product.variantId">
          <g-image :src="product.image.thumbnail" :alt="product.image.altText" />

          <div>
            <h4 v-if="product.variantTitle">{{ product.variantTitle }}</h4>
            <h4 v-else>{{ product.productTitle }}</h4>

            <p>Qty; {{ product.qty }}</p>
          </div>

          <p>
            Price:
            <span class="font-bold">{{ product.price.amount }}</span>
          </p>

          <button
            class="delete is-danger"
            @click="removeItem(product.variantId)"
            @keyup="removeItem(product.variantId)"
          >
            <small>Remove</small>
          </button>
        </div>
      </div>

      <div class="text-xl">
        Total:
        <span class="font-bold">{{ cartTotal }}</span>
      </div>

      <g-link
        class="button is-fullwidth"
        @click="close"
        to="/cart"
      >
        View Cart
      </g-link>

      <g-link
        class="button is-fullwidth is-primary"
        @click="close"
        to="/checkout"
      >
        Checkout &amp; Pay
      </g-link>
    </div>
  </DataCart>
</template>

<script>
import DataCart from "~/components/data/Cart";

export default {
  components: {
    DataCart
  },

  computed: {
    sidebar() {
      return this.$store.state.sidebar;
    }
  },

  methods: {
    close() {
      this.$store.commit("openSidebar", false);
    }
  }
};
</script>

<style lang="scss" scoped>
.sidebar {
  position: fixed;
  right: 0;
  bottom: 0;
  width: 20rem;
  background-color: #fff;
  border-left: 1px solid #eee;
  padding: 1rem;
  box-shadow: 0 0 1rem rgba(0, 0, 0, 0.15);
  top: 0;
  bottom: 0;
  z-index: 1000;
  transition: transform 0.3s ease-in-out;
}

.sidebar-offset {
  transform: translate(100%, 0);
}
</style>