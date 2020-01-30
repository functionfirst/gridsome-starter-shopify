<template>
  <Layout>
    <div
      v-if="collection"
      class="hero is-large">
      <div class="container">
        <div class="columns">
          <div class="column is-two-fifths header">
            <div class="header-content">
              <h1 class="title is-size-1 is-family-secondary">
                Our Core Collection
              </h1>
              <div
                v-html="collection.descriptionHtml"
                class="content" />
              <g-link
                :to="`collection/${collection.handle}`"
                class="button">
                Shop Now
              </g-link>
            </div>
          </div>
          <div class="column is-three-fifths">
            <figure class="image">
              <img
                :src="collection.image.src"
                :alt="collection.image.altText">
            </figure>
          </div>
        </div>
      </div>
    </div>
    <br>
    <br>

    <FeaturedProducts />
  </Layout>
</template>

<script>
import FeaturedProducts from '~/components/FeaturedProducts'
export default {
  metaInfo: {
    title: 'Come, shop!'
  },

  components: {
    FeaturedProducts
  },

  computed: {
    collection () { return this.$page.allShopifyCollection.edges.length && this.$page.allShopifyCollection.edges[ 0 ].node }
  }
}
</script>

<page-query>
query ShopifyProducts {
  allShopifyCollection (limit: 1) {
    edges {
      node {
        id
        handle
        title
        descriptionHtml
        image {
          altText
          src: transformedSrc(maxWidth: 800, maxHeight: 800, crop: CENTER)
        }
      }
    }
  }
}
</page-query>

<style lang="scss" scoped>
.header {
  align-items: center;
  display: flex;
  .header-content {
    text-align: center;
  }
}
</style>
