<template>
    <div class="pesanan-succes">
      <Navbar />
      <div class="container">
        <div class="row justify-content-center mt-5">
          <div class="col text-center">
            <img :src="'assets/img/wating.png'" alt="hero" width="300" />
            <h2><strong>Pesanan Sukses !!</strong></h2>
            <h4><strong>Pesanan anda segera diproses</strong><br>Mohon Menunggu</h4>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
export default {
  name: "pesanan-success",
  components: {
    Navbar,
  },
};
</script>

<style>
</style>