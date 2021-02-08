<template>
  <nav class="navbar navbar-expand-lg navbar-light">
    <div class="container">
      <a class="navbar-brand" href="#">Kuliner.id</a>
      <button
        class="navbar-toggler"
        type="button"
        data-toggle="collapse"
        data-target="#navbarTogglerDemo02"
        aria-controls="navbarTogglerDemo02"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" id="navbarTogglerDemo02">
        <ul class="navbar-nav mr-auto mt-2 mt-lg-0">
          <li class="nav-item">
            <router-link class="nav-link" to="/"> Home </router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/foods"> Foods </router-link>
          </li>
        </ul>

        <ul class="navbar-nav ml-auto mt-2 mt-lg-0">
          <li class="nav-item">
            <router-link class="nav-link" to="/keranjang"> 
            Keranjang
            <b-icon icon="basket2" aria-hidden="true"></b-icon> 
            <span class="badge badge-success ml-2">{{ updateKeranjang? updateKeranjang.length : jmlPesan.length }}</span>
            </router-link>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>

<script>
import axios from "axios";
export default {
  name: "navbar",

  data() {
    return {
      jmlPesan: [],
    };
  },
  props:['updateKeranjang'],
  methods: {
    setJmlPesanan(data) {
      this.jmlPesan = data;
    },

    getJmlKeranjang() {
    axios
      .get("http://localhost:3000/keranjangs/")
      .then((response) => this.setJmlPesanan(response.data))
      .catch((error) => console.log("Error log: " + error));
    },
},
  mounted() {
    this.getJmlKeranjang()
  },
};
</script>

<style>
</style>