<template>
  <div id="app">
    <h1>PETstock FED Test</h1>
    <div class="controls">

      <!-- Start filter buttons -->
      <p>Filter by category:</p>
      <section class="categories">
        <ul>
          <li v-for="category in categories" v-bind:key="category.slug">
            <button @click="selectCategory(category)">
              {{ category.name }}
            </button>
          </li>
          <li>
            <button @click="selectCategory(null)">
              View all
            </button>
          </li>
        </ul>
      </section>
    </div>
    <!-- End filter buttons -->

    <!-- Start results list --> 
    <div class="results">
      <p>Found <strong>{{ matchProducts.length }}</strong> results</p>

      <!-- Product -->
      <ul class="products">
        <li v-for="product in matchProducts" :key="product.key">

          <!-- Sale spot -->
          <div class="sale-spot" v-if="product.isOnSale">
            <p>Sale</p>
          </div>
          <!-- End sale spot -->

          <a v-bind:href="product.href" v-bind:title="product.name">
            <img v-bind:src="product.img" v-bind:alt="product.name">
          </a>
          <div class="details-wrapper">
            <div class="desc-wrapper">
              <a class="description" v-bind:href="product.href" v-bind:title="product.name">
                {{ product.name }}
              </a>
            </div>
            <p class="was-price" v-if="product.isOnSale">{{ 'Was $' + product.fullPrice }}</p>
            <p class="sale-price" v-if="product.isOnSale">NOW {{'$' + product.listPrice }}</p>
            <p class="price" v-else>{{'$' + product.listPrice }}</p>
          </div>
        </li>
      </ul>
      <!-- End product -->
    
    </div>
    <!-- End results list -->

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
    matchProducts() {
      return this.products.filter(product => (
        !this.selectedCategory || product.category === this.selectedCategory.slug 
      ));
    }
  },
  methods: {
    selectCategory(category) {
      this.selectedCategory = category;
    }
  },
  mounted() {
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
$color-controls: #3333FF;
$color-controls-hover: #0404a8;
$color-controls-active: #444444;

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
  .controls {
    display: grid;
    grid-template-columns: 100%;
  }
  .categories {
    ul {
      display: flex;
      flex-direction: row;
      width: 100%;
      height: 2.5rem;
      padding: 0.5rem;
      li {
        margin-right: 0.25rem;
        button {
          width: 11rem;
          padding: 5% 20%;
          color: $color-white;
          font-size: 1.2rem;
          text-align: center;
          cursor: pointer;
          background-color: $color-controls;
          border: none;
          border-radius: .3125rem;
          transition: background-color .33s ease-in-out;
        }
        button:hover {
          background-color: $color-controls-hover;
          }
        button:focus {
          background-color: $color-controls-active;
          outline: none;
          }
      }
    }
  }
  .results > ul {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    justify-content: space-evenly;
    grid-gap: 40px 10px;
  }
  .products {
    li {
      padding: 0.5rem;
      position: relative;
      .sale-spot {
        background-color: $color-sale;
        color: $color-white;
        text-align: center;
        display: block;
        font-size: 1.3rem;
        text-transform: uppercase;
        width: 4.5rem;
        height: 4.5rem;
        border-radius: 50%;
        z-index: 10;
        position: absolute;
        top: 0;
        right: 10%;
          p {
            margin: 1.4rem 0;
          }
      }
      img {
        max-width: 100%;
        height: auto;
      }
      a {
        text-decoration: none;
        color: $color-gray;
      }

      p {
        margin: 0.5rem 0;
      }
      .details-wrapper {
        padding: 8% 8%;
      }
      .desc-wrapper {
        min-height: 4.5rem;
      }
      .price { 
        color: $color-gray;
        font-weight: 700;
      }
      .was-price {
        color: $color-gray;
        font-size: 0.8rem;
      }
      .sale-price {
        color: $color-sale;
        font-weight: 700;
      }
    }
  }
  @media screen and (min-width: $size-mobile-max) {

  }
}
</style>
