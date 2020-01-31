<template>
  <Layout>
    <div class="flex justify-between items-center">
      <div>
        <g-link>
          &larr; Continue Shopping
        </g-link>
      </div>

      <h1 class="text-center text-4xl font-bold">
        Cart
      </h1>
      <div>
        Secure Checkout
      </div>
    </div>

    <div
      v-if="!cart.length"
      class="text-center my-4"
    >
      <h3 class="my-12 text-3xl">
        You currently have nothing in your cart
      </h3>

      <g-link
        to="/"
        class="border px-3 py-2 text-center rounded hover:bg-gray-100 hover:borer-gray-300">
        Browse
      </g-link>
    </div>

    <div
      class="flex items-start relative"
      v-if="cart.length"
    >
      <div class="flex-1 mr-12">
        <div
          class="flex border-t border-b -mb-px py-4 items-stretch justify-between"
          v-for="item in cart"
          :key="item.variantId"
        >
          <figure class="w-32 mr-8">
            <g-image
              :src="item.image.thumbnail"
              :alt="item.image.altText || item.title"
            />
          </figure>

          <div class="flex-1">
              <g-link
                :to="`/product/${ item.handle }`"
                class="font-bold"
              >
                {{ item.productTitle }}
                {{ item.variantTitle !== 'Default Title' ? `- ${item.variantTitle}` : '' }}
              </g-link>

              <p>
                Quantity: {{ item.qty }}
              </p>
          </div>
          <div class="flex flex-col justify-between">
             <button
                class="underline hover:text-gray-700 hover:no-underline"
                @click="removeItem(item.variantId)"
                @keyup="removeItem(item.variantId)">
                <small>Remove</small>
              </button>

            {{ totalPrice(item) }}
          </div>
        </div>
      </div>

      <div
        class="border p-8 w-1/3 sticky top-0"        
      >
        <div class="border-t text-xl border-b py-4 flex justify-between items-center">
          Estimated Total

          <span class="font-bold">{{ cartTotal }}</span>
        </div>

        <form
          class="mt-4"
          @submit.prevent="checkout"
        >
          <label for="email" class="block cursor-pointer my-2">Enter your email address to checkout</label>

          <input
            id="email"
            v-model="email"
            class="border my-2 px-3 py-2 w-full"
            type="email"
            placeholder="Your email address"
            required
          >

          <button
            :class="{'is-loading': isLoading}"
            type="submit"
            class="w-full rounded mt-2 bg-teal-700 p-4 text-white text-lg hover:bg-teal-800"
          >
            Checkout
          </button>
        </form>
      </div>
    </div>
  </Layout>
</template>

<script>
import currency from 'currency.js'
import gql from 'graphql-tag'
export default {
  metaInfo: {
    title: 'Your Cart'
  },
  data: () => ({ email: '', isLoading: false }),
  computed: {
    cart () { return this.$store.state.cart },
    cartTotal () {
      const total = this.cart.reduce((total, item) => total.add(currency(item.price.amount).multiply(item.qty)), currency(0, { formatWithSymbol: true, symbol: '£' }))
      return total.format()
    }
  },
  methods: {
    totalPrice ({ qty, price }) {
      return currency(price.amount, { formatWithSymbol: true, symbol: '£' }).multiply(qty).format()
    },
    async removeItem (itemId) {
      await this.$store.commit('removeFromCart', itemId)
      this.$notify({
        title: 'Item removed from cart',
        type: 'primary'
      })
    },
    async checkout () {
      const email = this.email
      if (!this.cart.length) return alert('No items in cart')
      const lineItems = this.cart.map(item => ({ quantity: item.qty, variantId: item.variantId }))

      const checkoutInput = { email, lineItems }

      try {
        this.isLoading = true
        const { data: { checkoutCreate } } = await this.$apollo.mutate({
          mutation: gql`mutation checkoutCreate($input: CheckoutCreateInput!) {
            checkoutCreate(input: $input) {
              checkout {
                id
                webUrl
              }
              checkoutUserErrors {
                code
                field
                message
              }
            }
          }
          `,
          variables: { input: checkoutInput }
        })
        if (checkoutCreate.checkoutUserErrors.length) throw new Error(checkoutCreate.checkoutUserErrors[ 0 ].message)

        window.location = checkoutCreate.checkout.webUrl
      } catch (error) {
        this.isLoading = false
        console.error(error)
        this.$notify({
          title: 'Whoops...',
          type: 'danger',
          message: 'Something went wrong - please try again.'
        })
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.input {
  height: 50px !important;
}
</style>
