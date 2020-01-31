<template>
  <header>
    <div class="mx-2">
      <div class="flex flex-wrap justify-between items-center max-w-6xl mx-auto mt-2">
        <button class="w-10 h-10 p-1 lg:hidden mr-4 text-xs leading-none">
          <span class="block h-1 rounded bg-black mb-1" aria-hidden="true" />
          <span class="block h-1 rounded bg-black mb-1" aria-hidden="true" />
          <span class="block h-1 rounded bg-black mb-1" aria-hidden="true" />
          Menu
        </button>

        <g-link
          to="/"
          class="font-bold text-xl"
        >
          {{ $static.metadata.siteName }}
        </g-link>

        <button
          @click="open"
          class="relative lg:order-1 text-sm border py-2 px-3 rounded hover:border-gray-400 hover:bg-gray-100"
        >
          My Cart
          <span
            v-if="cart.length"
            class="absolute -mt-2 -mr-2 bg-teal-700 text-white top-0 right-0 rounded-full text-xs h-6 w-6 flex items-center justify-center"
          >
            {{ cart.length }}
          </span>
        </button>

          <div class="w-full lg:w-auto flex-initial lg:flex-1 lg:max-w-lg justify-between items-center my-2">
            <form class="flex" method="post" @submit.prevent="">
              <input
                id="search"
                v-model="searchTerm"
                class="rounded-l w-full border py-2 px-3"
                type="text"
                placeholder="Search"
              >

              <button class="bg-black text-white px-3 py-2 rounded-r">
                Search
              </button>
            </form>

            <div class="relative hidden">
              <div class="flex flex-col overflow-hidden absolute shadow-lg z-10 w-full h-64">
                <div class="flex-1 bg-white ">
                  <g-link
                    v-for="result in searchResults"
                    :key="result.id"
                    :to="result.path"
                  >
                    {{ result.title }}
                  </g-link>
                </div>

                <g-link :to="`/search?term=${searchTerm}`" class="bg-white rounded-b text-center px-3 py-2 border-t hover:bg-gray-100">
                  View more results
                </g-link>
              </div>
            </div>
          </div>
      </div>
    </div>

    <div class="bg-black text-white">
      <nav
        class="flex justify-between text-center uppercase max-w-6xl mx-auto"
        role="navigation"
        aria-label="main navigation"
      >
        <g-link to="/" class="flex-1 px-3 py-2 hover:bg-gray-800">
          Home
        </g-link>

        <g-link to="/collections" class="flex-1 px-3 py-2 hover:bg-gray-800">
          Collections
        </g-link>

        <g-link to="/contact" class="flex-1 px-3 py-2 hover:bg-gray-800">
          Contact
        </g-link>

        <g-link to="/about" class="flex-1 px-3 py-2 hover:bg-gray-800">
          About
        </g-link>
      </nav>
    </div>
  </header>
</template>

<script>
export default {
  data: () => ({
    searchTerm: ''
  }),

  computed: {
    cart () { return this.$store.state.cart },
    searchResults () {
      if (!this.$search) return []
      const searchTerm = this.searchTerm
      if (searchTerm.length < 3) return []
      return this.$search.search({ query: searchTerm, limit: 5, suggest: true })
    }
  },

  methods: {
    open () {
      this.$store.commit("openSidebar", true);
    }
  },

  watch: {
    $route (to, from) {
      this.searchTerm = ''
    }
  }
}
</script>

<static-query>
query {
  metadata {
    siteName
  }
}
</static-query>