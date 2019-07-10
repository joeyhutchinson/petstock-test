<template>
  <div id="app">
    <h1>PETstock FED Test</h1>
    <div class="controls">
      <p>Filter by category:</p>
      <section class="categories">
        <ul>
          <li v-for="category in categories" v-bind:key="category.slug">
            <button>
              {{ category.name }}
            </button>
          </li>
          <li>
            <button>
              View all
            </button>
          </li>
        </ul>
      </section>
    </div>
    <div class="results">
      <p>Found <strong>{{ products.length }}</strong> results</p>
      <ul class="products">
        <li v-for="product in products" :key="product.sku">
          <a href="#" title="Product 1">
            {{ product.name }}
          </a>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import response from './data.json';
export default {
  name: 'app',
  data() {
    return {
      /** @type {Array} Category response. */
      categories: [],
      /** @type {Array} Product response. */
      products: [],
      /** @type {String} Selected viewing category or null. */
      selectedCategory: null,
    };
  },
  computed: {
  },
  methods: {

  },
  mounted () {
    setTimeout(() => {
      this.categories = response.categories;
      this.products = response.products.map(product => ({
        ... product,
        isOnSale: product.listPrice !== product.fullPrice,
      }));
    }, 250);
  }
};
</script>

<style lang="scss">
$size-mobile-max: 37.5rem;
$size-large: 2 * $size-mobile-max;

$color-gray: #6B6B6B;
$color-white: #fefefe;
$color-sale: #82B366;
$color-controls: #3333FF;

#app {
  font-family: Helvetica, Arial, sans-serif;
  > * {
    max-width: 75rem;
    margin: 0 auto;
  }
  ul {
    list-style-type: none;
    padding: 0;
    margin: 0;
  }
  .categories {
    ul {
      display: flex;
      li {
        margin-right: 0.25rem;
      }
    }
  }
  .controls {
    display: grid;
    grid-template-columns: 3fr 2fr;
  }
  .controls button {
    border: 1px solid $color-controls;
    background-color: $color-white;
  }
  .results > ul {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
  }
  @media screen and (min-width: $size-mobile-max) {

  }
}
</style>
