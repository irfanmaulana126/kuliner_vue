<template>
  <div class="productcard">
    <div class="card shadow">
      <img class="card-img-top" :src=" 'assets/img/' + product.gambar " alt="Card image cap" />
      <div class="card-body">
        <h5 class="card-title">{{product.nama}}</h5>
        <p class="card-text">
            Harga : RP. {{product.harga}}
        </p>
        <router-link class="btn btn-success" :to="'/foods/'+product.id"><b-icon-cart></b-icon-cart> Pesan</router-link>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CardProduct",
  props: ["product"],
};
</script>

<style>
</style>