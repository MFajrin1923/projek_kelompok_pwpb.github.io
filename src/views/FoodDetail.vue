<template>
  <div class="form-detail">
    <Navbar />
    <div class="container">
      <!-- Breadcrumb -->
      <div class="row mt-5">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link class="exact" to="/">Home</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link class="exact" to="/foods">Foods</router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">Data</li>
            </ol>
          </nav>
        </div>
      </div>
      <!-- End Breadcrumb -->

      <div class="row mt-3 mb-5">
        <div class="col-md-6">
          <img
            :src="'../assets/images/' + products.gambar"
            alt="makanan"
            class="img-fluid shadow"
          />
        </div>
        <div class="col-md-6">
          <h2>
            <strong>{{ products.nama }}</strong>
          </h2>
          <hr />
          <h4>
            Harga <strong>Rp. {{ products.harga }}</strong>
          </h4>

          <form class="mt-3" v-on:submit.prevent>
            <div class="form-group">
              <label for="jumlah-pesanan">Jumlah Pesanan</label>
              <select
                class="form-control"
                id="jumlah-pesanan"
                v-model="pesan.jumlah_pemesanan"
              >
                <option>1</option>
                <option>2</option>
                <option>3</option>
                <option>4</option>
                <option>5</option>
              </select>
            </div>
            <div class="form-group">
              <label for="keterangan">Keterangan</label>
              <textarea
                v-model="pesan.keterangan"
                name="keterangan"
                id="keterangan"
                class="form-control"
                placeholder="Keterangan seperti : pedas, tanpa bawang"
              ></textarea>
            </div>
            <button type="submit" class="btn btn-danger" @click="pemesanan">
              <b-icon-cart></b-icon-cart> Pesan
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
  name: "FoodDetail",
  components: {
    Navbar,
  },
  data() {
    return {
      products: {},
      pesan: {},
    };
  },
  methods: {
    setProduct(data) {
      this.products = data;
    },
    pemesanan() {
      this.pesan.products = this.products;
      if (this.pesan.jumlah_pemesanan) {
        axios
          .post("http://localhost:3000/keranjangs", this.pesan)
          .then(() => {
            this.$router.push({ path: "/keranjang" });
            this.$toast.success("Sukses Masuk Keranjang", {
              // optional options Object
              position: "top-right",
            });
          })
          .catch((error) => console.log(error));
      }
      if (!this.pesan.jumlah_pemesanan) {
        this.$toast.error("Silahkan isi jumlah pesanan !", {
          position: "top-right",
        });
      }
    },
  },
  mounted() {
    axios("http://localhost:3000/products/" + this.$route.params.id)
      .then((response) => this.setProduct(response.data))
      .catch((error) => console.log(error));
  },
};
</script>

<style>
</style>