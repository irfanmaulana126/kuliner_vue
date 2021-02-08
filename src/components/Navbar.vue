<template>
  <div>
    <b-navbar toggleable="lg" type="light">
      <div class="container">
        <b-navbar-brand href="#">Kulineran</b-navbar-brand>

        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <li class="nav-item">
              <router-link class="nav-link" to="/"> Home </router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/foods"> Foods </router-link>
            </li>
          </b-navbar-nav>

          <!-- Right aligned nav items -->
          <b-navbar-nav class="ml-auto">
            <li class="nav-item">
              <router-link class="nav-link" to="/keranjang">
                Keranjang
                <b-icon icon="basket2" aria-hidden="true"></b-icon>
                <span class="badge badge-success ml-2">{{
                  updateKeranjang ? updateKeranjang.length : jmlPesan.length
                }}</span>
              </router-link>
            </li>
          </b-navbar-nav>
        </b-collapse>
      </div>
    </b-navbar>
  </div>
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
  props: ["updateKeranjang"],
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
    this.getJmlKeranjang();
  },
};
</script>

<style>
</style>