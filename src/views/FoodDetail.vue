<template>
  <div class="foods-detail">
    <Navbar />
    <div class="container">
      <div class="row mt-5">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/" class="text-dark"> Home </router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/foods" class="text-dark"> Foods </router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">
                {{ productDetail.nama }}
              </li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row mt-4">
        <div class="col-md-6">
          <img
            :src="'/assets/img/' + productDetail.gambar"
            class="img-fluid shadow"
          />
        </div>
        <div class="col-md-6">
          <h3>
            <strong>{{ productDetail.nama }}</strong>
          </h3>
          <hr />
          <label for="Harga"
            >Harga : <strong>{{ productDetail.harga }}</strong></label
          ><br />
          <form action="" method="post" class="mt-4" v-on:submit.prevent>
            <div class="form-group">
              <label for="jmlPesan">Jumlah Pesanan</label>
              <input
                type="number"
                class="form-control"
                v-model="pesanan.jumlah"
              />
            </div>
            <div class="form-group">
              <label for="keterangan">Keterangan</label>
              <textarea
                v-model="pesanan.keterangan"
                class="form-control"
                placeholder="exp: Pedas, Manis, dsb"
                id=""
                cols="5"
                rows="5"
              ></textarea>
            </div>
            <button type="submit" class="btn btn-success" @click="pemesanan()">
              <b-icon-cart></b-icon-cart> send Chart
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import axios from "axios";

export default {
  name: "FoodsDetail",
  components: {
    Navbar,
  },
  data() {
    return {
      productDetail: [],
      pesanan: {},
    };
  },
  methods: {
    setDetailProduct(data) {
      this.productDetail = data;
    },
    pemesanan (){
        if (this.pesanan.jumlah) {        
            this.pesanan.products = this.productDetail;
            axios
            .post("http://localhost:3000/keranjangs/", this.pesanan)
            .then(() => {
                this.$router.push({path:"/keranjang"});
                this.$toast.success('Masuk Keranjang', {
                    type:'success',
                    position:'top-right',
                    duration:5000,
                    dismissible:true
                })
            })
            .catch((error) => console.log("Error log: " + error));
        }else{
            this.$toast.error('Harap isi jumlah', {
                    type:'error',
                    position:'top-right',
                    duration:5000,
                    dismissible:true
                })
        }
    }
},
  mounted() {
    axios
      .get("http://localhost:3000/products/" + this.$route.params.ids)
      .then((response) => this.setDetailProduct(response.data))
      .catch((error) => console.log("Error log: " + error));
  },
};
</script>

<style>
</style>