<script>
export default {
  name: "ProductDisplay",
  data() {
    return {
      productId: 0,
      isLoading: false,
      setRating: 0,
      setRatingEmpty: 0,
      productUnavailable: 0,
      products: {
        data: {
          category: "",
        },
      },
    };
  },
  methods: {
    //fetch data dari API
    async callProductApi() {
      const api = await fetch(
        `https://fakestoreapi.com/products/${this.productId}`
      );
      const response = await api.json();
      //   console.log(response);
      return response;
      //   const data = response;
      //   this.productData = data;
    },

    //dapetin nomor product ID
    async getProductId() {
      this.isLoading = true;

      if (this.productId !== 20) {
        this.productId++;
      } else {
        this.productId = 1;
      }

      let data = await this.callProductApi();

      if (
        data.category === "men's clothing" ||
        data.category === "women's clothing"
      ) {
        this.products = { data };
        this.productUnavailable = true;
        this.setRating = Math.round(data.rating.rate);
        // this.setRatingEmpty = 5 - this.setRating;
      } else {
        this.products = {};
        this.productUnavailable = false;
      }
      this.isLoading = false;
    },
  },
  //call method pas di refresh
  mounted() {
    this.getProductId();
  },
};
</script>

<template>
  <!-- //container -->
  <div class="container bg-netral">
    <!-- loading spinner -->
    <div v-if="isLoading" class="card">
      <!-- mulai cek kalo product unavailable -->
      <div v-if="!productUnavailable" class="product-unavailable-container">
        <div class="product-text">
          <div class="cta">
            <div class="loader"></div>
          </div>
        </div>
      </div>
      <!-- selesai cek product unavailable -->

      <!-- load product start -->
      <!-- <div v-else class="product-container load">
        <div class="load-thumbnail bg-load"></div>
        <div class="load-details">
          <div class="load-text-details">
            <div class="load-text-title bg-load"></div>
            <div class="load-text-sub-title">
              <div class="load-category bg-load"></div>
              <div class="load-rating bg-load"></div>
            </div>
            <div class="load-text-description bg-load"></div>
          </div>
          <div class="load-details-action">
            <div class="load-price bg-load"></div>
            <div class="load-cta">
              <div class="load-cta-buy bg-load"></div>
              <div class="load-cta-next bg-load"></div>
            </div>
          </div>
        </div>
      </div> -->
      <div v-else class="loader">
        <div class="loader"></div>
      </div>

      <!-- loading selesai -->
    </div>

    <!-- tampilan test -->

    <div
      v-else
      :class="{
        container: true,
        'bg-unavailable': !productUnavailable,
        'bg-men-color':
          products.data && products.data.category === 'men\'s clothing',
        'bg-women-color':
          products.data && products.data.category === 'women\'s clothing',
      }"
    >
      <div class="overlay">
        <img src="../assets/bg-pattern.svg" alt="bg-image" />
      </div>
      <div class="card">
        <!-- Unavailable Product Start-->
        <div v-if="!productUnavailable" class="product-unavailable-container">
          <div class="overlay">
            <img src="../assets/sad-face.png" alt="bg-unvailable-product" />
          </div>
          <div class="product-text">
            <p>This product is Unvailable to show</p>
            <div class="cta">
              <button class="cta-next" type="button" @click="getProductId()">
                Next Product
              </button>
            </div>
          </div>
        </div>

        <!-- tampilan test selsai -->

        <!-- show products by category -->

        <!-- Product by Category Start -->
        <div v-else class="product-container">
          <div class="product-image">
            <img :src="products.data.image" alt="image-product" />
          </div>
          <div class="product-text">
            <div class="details">
              <h4
                :class="{
                  title: true,
                  'font-men': products.data.category === 'men\'s clothing',
                  'font-women': products.data.category === 'women\'s clothing',
                }"
              >
                {{ products.data.title }}
              </h4>
              <div class="sub-title">
                <span>{{ products.data.category }}</span>
                <div class="rating">
                  <span>{{ products.data.rating.rate }}/5 </span>
                  <div class="rating">
                    <div
                      v-for="setRate in setRating"
                      :key="setRate"
                      :class="{
                        circle: true,
                        'bg-navy': products.data.category === 'men\'s clothing',
                        'bg-magenta':
                          products.data.category === 'women\'s clothing',
                      }"
                    ></div>

                    <div
                      v-for="setRateEmpty in setRatingEmpty"
                      :key="setRateEmpty"
                      :class="{
                        circleborder: true,
                        'border-men':
                          products.data.category === 'men\'s clothing',
                        'border-women':
                          products.data.category === 'women\'s clothing',
                      }"
                    ></div>
                  </div>
                </div>
              </div>
              <div class="description">
                <p>{{ products.data.description }}</p>
              </div>
            </div>
            <div class="action">
              <span
                :class="{
                  price: true,
                  'font-men': products.data.category === 'men\'s clothing',
                  'font-women': products.data.category === 'women\'s clothing',
                }"
              >
                $ {{ products.data.price }}</span
              >
              <div class="cta">
                <button
                  :class="{
                    'cta-buy': true,
                    'bg-navy btn-buy-men':
                      products.data.category === 'men\'s clothing',
                    'bg-magenta':
                      products.data.category === 'women\'s clothing',
                  }"
                  type="button"
                >
                  Buy Now
                </button>
                <button
                  :class="{
                    'cta-next': true,
                    'font-men border-men btn-men':
                      products.data.category === 'men\'s clothing',
                    'font-women border-women btn-women':
                      products.data.category === 'women\'s clothing',
                  }"
                  type="button"
                  @click="getProductId()"
                >
                  Next Product
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- Product by Category -->
    </div>

    <!-- kategori selesai -->
  </div>
</template>

<style>
@import "../assets/style/page.css";
</style>
