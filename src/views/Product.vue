<template>
  <div class="Product">
    <HeaderShayna />

    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="breadcrumb-text product-more text-left">
              <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
              <span>Detail</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="row">
              <div class="col-lg-6">
                <div class="product-pic-zoom">
                  <img class="product-big-img" :src="primary_photo" alt="" />
                </div>
                <div class="product-thumbs" v-if="productDetails.galleries.length > 0">
                  <carousel
                    class="product-thumbs-track ps-slider"
                    :margin="14"
                    :dots="false"
                    :items="3"
                    :nav="false"
                  >
                    <div
                    v-for="ss in productDetails.galleries"
                    :key="ss.id"
                      class="pt"
                      @click="changeImage(ss.photo)"
                      :class="ss.photo == primary_photo ? 'active' : ''"
                    >
                      <img :src="ss.photo" alt="" />
                    </div>

                    <!-- <div class="pt" @click="changeImage(thumbs[1])" :class="thumbs[1] == primary_photo ? 'active' : '' ">
                                        <img src="img/mickey2.jpg" alt="" />
                                    </div>

                                    <div class="pt" @click="changeImage(thumbs[2])" :class="thumbs[2] == primary_photo ? 'active' : '' ">
                                        <img src="img/mickey3.jpg" alt="" />
                                    </div>

                                    <div class="pt" @click="changeImage(thumbs[3])" :class="thumbs[3] == primary_photo ? 'active' : '' ">
                                        <img src="img/mickey4.jpg" alt="" />
                                    </div> -->
                  </carousel>
                </div>
              </div>
              <div class="col-lg-6">
                <div class="product-details text-left">
                  <div class="pd-title">
                    <span>{{ productDetails.type }}</span>
                    <h3>{{ productDetails.name }}</h3>
                  </div>
                  <div class="pd-desc">
                    <br>
                    <p v-html="productDetails.description">           
                    </p>
                    <h4>${{ productDetails.price }}</h4>
                  </div>
                  <div class="quantity">
                    <router-link to="/cart"> 
                    <a @click="saveKeranjang(productDetails.id, productDetails.name, productDetails.price, productDetails.galleries[0].photo)" href="#" class="primary-btn pd-cart">Add To Cart</a>
                    </router-link>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- Product Shop Section End -->

    <RelatedProduct />

    <FooterShayna />
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from "@/components/HelloWorld.vue";
import HeaderShayna from "@/components/HeaderShayna.vue";
import RelatedProduct from "@/components/RelatedProduct.vue";
import FooterShayna from "@/components/FooterShayna.vue";
import carousel from "vue-owl-carousel";
import axios from "axios";

export default {
  name: "Product",
  components: {
    HeaderShayna,
    RelatedProduct,
    FooterShayna,
    carousel,
  },
  data() {
    return {
      primary_photo: "",
      productDetails: [],
      keranjangUser: []
    };
  },
  methods: {
    changeImage(urlImage) {
      this.primary_photo = urlImage;
    },
    setDataPicture(data) {
      this.productDetails = data;
      this.primary_photo = data.galleries[0].photo;
    },
    saveKeranjang(idProduct, nameProduct, priceProduct, photoProduct){
        
        var productStored = {
        "id": idProduct,
        "name": nameProduct,
        "price": priceProduct,
        "photo": photoProduct
        }

      this.keranjangUser.push(productStored);
      const parsed = JSON.stringify(this.keranjangUser);
      localStorage.setItem('keranjangUser', parsed);
    }
  },

  mounted() {
     if (localStorage.getItem('keranjangUser')) {
      try {
        this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'));
      } catch(e) {
        localStorage.removeItem('keranjangUser');
      }
    }
    axios
      .get("https://shayna-backend.tikoaprilianto.my.id/api/products", {
        params: {
          id: this.$route.params.id,
        },
      })
      .then((res) => this.setDataPicture(res.data.data))

      .catch((err) => console.log(err));
  },
};
</script>
