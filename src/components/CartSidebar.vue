<template>
  <DataCart v-slot="{ cart, cartTotal, removeItem }">
    <div>
      <div
        class="flex flex-col w-11/12 md:w-1/2 lg:w-1/3 xl:w-1/4 fixed inset-y-0 border-l z-20 right-0 bg-white transition-transform transition-300 transition-ease-in-out"
        :class="sidebar ? '' : 'translate-100'"
      >
        <div class="flex items-center justify-between border-b border-gray-300">
          <div></div>
          <div class="text-center text-lg">
            Added Items
            ({{ cart.length }})
          </div>

          <button @click="close" class="h-12 w-12">&times;</button>
        </div>

        <div class="flex flex-col flex-1 max-h-full">
          <template v-if="cart.length">
            <div class="flex flex-col flex-1 overflow-auto p-4">
              <div
                v-for="product in cart"
                :key="product.variantId"
                class="flex w-full justify-between items-start mb-4 border-b pb-4"
              >
                <div class="w-24 mr-4" v-if="product.image">
                  <g-image :src="product.image.thumbnail" :alt="product.image.altText" />
                </div>

                <div class="flex-1">
                  <g-link class="borderblock" :to="`product/${product.handle}`">
                    {{ product.productTitle }}
                    {{ product.variantTitle !== 'Default Title' ? `- ${item.variantTitle}` : '' }}
                  </g-link>

                  <div class="flex text-sm justify-between">
                    <div>
                      <p>Qty: <span class="font-bold">{{ product.qty }}</span></p>

                      <button
                        class="underline hover:text-gray-800"
                        @click="removeItem(product.variantId)"
                        @keyup="removeItem(product.variantId)"
                      >
                        <small>Remove</small>
                      </button>
                    </div>

                    <p class="self-end">
                      <span class="font-bold">{{ product.price.amount }}</span>
                    </p>
                  </div>
                </div>
              </div>
            </div>

            <div class="border-t border-b flex items-center justify-between m-4 p-4">
              Total
              <span class="font-bold">{{ cartTotal }}</span>
            </div>

            <g-link
              class="rounded border mx-4 px-3 py-2 text-center hover:border-gray-400"
              @click="close"
              to="/cart"
            >
              View Cart
            </g-link>

            <g-link
              class="rounded bg-green-900 mb-2 mx-4 px-3 py-2 text-white text-center mt-4 hover:bg-green-800"
              @click="close"
              to="/checkout"
            >
              Checkout &amp; Pay
            </g-link>
          </template>

          <p v-else class="text-center text-gray-900 p-4">
            Your cart is currently empty
          </p>
        </div>
      </div>

      <div v-show="sidebar" class="z-10 opacity-25 bg-black inset-0 fixed cursor-pointer" @click="close"></div>
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
.transition-transform {
  transition-property: transform
}

.transition-300 {
  transition-duration: 0.3s;
}

.transition-ease-in-out {
  transition-timing-function: ease-in-out
}

.translate-100 {
  transform: translate(100%, 0);
}
</style>