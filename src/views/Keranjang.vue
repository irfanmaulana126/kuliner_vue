<template>
  <div class="keranjang">
    <Navbar :updateKeranjang="keranjangs" />
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
                Keranjang
              </li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row">
        <div class="col">
          <h2>Keranjang <strong>Saya</strong></h2>
          <div class="table-responsive mt-4">
            <table class="table table-striped">
              <thead>
                <tr>
                  <th scope="col">#</th>
                  <th scope="col">Foto</th>
                  <th scope="col">Makanan</th>
                  <th scope="col">Jumlah</th>
                  <th scope="col">Keterangan</th>
                  <th scope="col">Harga</th>
                  <th scope="col">Total Harga</th>
                  <th scope="col">Hapus</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="(keranjang, index) in keranjangs"
                  :key="keranjang.id"
                >
                  <th>{{ index + 1 }}</th>
                  <td>
                    <img
                      :src="'/assets/img/' + keranjang.products.gambar"
                      class="img-fluid shadow"
                      width="250"
                    />
                  </td>
                  <td>{{ keranjang.products.nama }}</td>
                  <td>{{ keranjang.jumlah }}</td>
                  <td>
                    {{ keranjang.keterangan ? keranjang.keterangan : "-" }}
                  </td>
                  <td>Rp. {{ keranjang.products.harga }}</td>
                  <td>Rp. {{ keranjang.products.harga * keranjang.jumlah }}</td>
                  <td align="center" class="text-danger">
                    <button
                      class="btn btn-danger"
                      @click="HapusPesanan(keranjang.id)"
                    >
                      <b-icon-trash></b-icon-trash>
                    </button>
                  </td>
                </tr>
                <tr>
                  <td colspan="6" align="right">
                    <strong>Total Harga :</strong>
                  </td>
                  <td>
                    <strong>RP. {{ totalHarga }}</strong>
                  </td>
                  <td></td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>

      <!-- Form Checkout -->
      <div class="row justify-content-end">
        <div class="col-md-4">
          <form action="" method="post" class="mt-4" v-on:submit.prevent>
            <div class="form-group">
              <label for="jmlPesan">Nama Pemesan</label>
              <input
                type="text"
                class="form-control"
                v-model="pesanan.nama"
              />
            </div>
            <div class="form-group">
              <label for="keterangan">Nomor Meja</label>
              <input
                type="number"
                class="form-control"
                v-model="pesanan.NoMeja"
              />
            </div>
            <button type="submit" class="btn btn-success float-right" @click="checkout()">
               Checkout
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
  name: "Keranjang",
  components: {
    Navbar,
  },
  data() {
    return {
      keranjangs: [],
      pesanan: {},
    };
  },
  methods: {
    setKeranjangs(data) {
      this.keranjangs = data;
    },
    HapusPesanan(id) {
      axios
        .delete("http://localhost:3000/keranjangs/" + id)
        .then(() => {
          this.getKeranjang();
          this.$toast.error("Berhasil di hapus", {
            type: "error",
            position: "top-right",
            duration: 5000,
            dismissible: true,
          });
        })
        .catch((error) => console.log("Gagal: ", error));
    },
    checkout(){
      if (this.keranjangs.length > 0) {
        if (this.pesanan.nama && this.pesanan.NoMeja) {
          this.pesanan.keranjangs = this.keranjangs;
          axios
        .post("http://localhost:3000/pesanans/",this.pesanan)
        .then(() => {
          // hapus semua keranjang
          this.keranjangs.map(function(item) {
           return axios
              .delete("http://localhost:3000/keranjangs/" + item.id)
              .catch((error) => console.log("Gagal: ", error));
          })

          this.$router.push({path:"/pesanan-success"})
          this.$toast.success("Succes di pesan", {
            type: "success",
            position: "top-right",
            duration: 5000,
            dismissible: true,
          });
        })
        .catch((error) => console.log("Gagal: ", error));
          
        } else {
          this.$toast.error("Nama dan Nomer Meja harus di isi", {
            type: "error",
            position: "top-right",
            duration: 5000,
            dismissible: true,
          });
        }        
      }else{
        this.$toast.error("Harap Pilih produk terlebih dahulu", {
            type: "error",
            position: "top-right",
            duration: 5000,
            dismissible: true,
          });
      }
    },
    getKeranjang() {
      axios
        .get("http://localhost:3000/keranjangs")
        .then((response) => this.setKeranjangs(response.data))
        .catch((error) => console.log("Gagal: ", error));
    },
  },
  mounted() {
    this.getKeranjang();
  },
  computed: {
    totalHarga() {
      return this.keranjangs.reduce(function (items, data) {
        return items + data.products.harga * data.jumlah;
      }, 0);
    },
  },
};
</script>

<style>
</style>