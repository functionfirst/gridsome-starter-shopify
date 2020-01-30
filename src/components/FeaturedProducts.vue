<template>
  <div class="container has-text-centered">
      <hr>
      <h3 class="title is-size-4 is-family-secondary">
        Featured Products
      </h3>
      <hr>
      <br>
      <div class="columns is-multiline">
        <div
          v-for="({ node: product }) in featuredProducts"
          :key="product.id"
          class="column is-4">
          <div class="card">
            <div class="card-image">
              <figure class="image is-4by3">
                <img
                  :src="product.images[0].src"
                  :alt="product.images[0].altText || product.title">
              </figure>
            </div>
            <div class="card-content has-text-left">
              <div class="media">
                <div class="media-content">
                  <p class="title is-4 is-family-secondary">
                    {{ product.title }}
                  </p>
                  <p class="subtitle is-6">
                    {{ product.priceRange.minVariantPrice.amount }}
                  </p>
                </div>
              </div>

              <div
                v-html="product.descriptionHtml"
                class="content" />
              <div class="field is-grouped is-grouped-right">
                <div class="control">
                  <g-link
                    :to="`product/${product.handle}`"
                    class="button is-primary is-outlined">
                    View Product
                  </g-link>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
export default {
  computed: {
    featuredProducts () { return this.$static.allShopifyProduct.edges }
  }
}
</script>

<static-query>
query ShopifyProducts {
  allShopifyProduct (limit: 6) {
    edges {
      node {
        id
        title
        handle
        descriptionHtml
        priceRange {
          minVariantPrice {
            amount(format: true)
          }
        }
        images (limit: 1) {
          id
          altText
          src: transformedSrc (maxWidth: 400, maxHeight: 300, crop: CENTER)
        }
      }
    }
  }
}
</static-query>