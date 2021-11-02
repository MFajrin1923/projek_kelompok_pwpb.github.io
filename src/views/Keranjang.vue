<template>
  <div class="keranjang">
    <Navbar :updateKeranjang="keranjangs" />
    <div class="container">
      <!-- breadcrumb -->
      <div class="row mt-4">
        <div class="col-md-6">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link class="exact" to="/">Home</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/foods">Foods</router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">Data</li>
            </ol>
          </nav>
        </div>
        <div
          v-if="keranjangs.length > 1"
          class="col-md-6 d-flex justify-content-end"
        >
          <div class="mt-2">
            <a href="#nama" class="btn btn-danger mr-md-3">Bayar</a>
          </div>
        </div>
      </div>
      <!-- end breadcrumb -->
      <div v-if="keranjangs == 0" class="text-center">
        <div class="text-center d-none d-md-block">
          <img src="../assets/images/keranjangKosong.png" alt="" width="45%" />
        </div>
        <div class="text-center d-sm-block d-md-none">
          <img src="../assets/images/keranjangKosong.png" alt="" width="100%" />
        </div>
        <h1>Keranjang Kosong</h1>
      </div>
      <div v-else class="table-responsive mt-3">
        <table class="table table-hover">
          <thead>
            <tr>
              <th scope="col">#</th>
              <th scope="col">Foto</th>
              <th scope="col">Makanan</th>
              <th scope="col">Keterangan</th>
              <th scope="col">Jumlah</th>
              <th scope="col">Harga</th>
              <th scope="col">Total Harga</th>
              <th align="center" scope="col">Hapus</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(keranjang, index) in keranjangs" :key="keranjang.id">
              <th scope="row">{{ index + 1 }}</th>
              <td>
                <img
                  :src="'assets/images/' + keranjang.products.gambar"
                  alt="makanan"
                  class="img-fluid shadow"
                  width="250"
                />
              </td>
              <td>{{ keranjang.products.nama }}</td>
              <td>{{ keranjang.keterangan ? keranjang.keterangan : "-" }}</td>
              <td align="center">{{ keranjang.jumlah_pemesanan }}</td>
              <td>Rp. {{ keranjang.products.harga }}</td>
              <td>
                <strong
                  >Rp.
                  {{
                    keranjang.products.harga * keranjang.jumlah_pemesanan
                  }}</strong
                >
              </td>
              <td align="center">
                <b-icon
                  icon="trash"
                  style="color: #f50057; cursor: pointer"
                  @click="hapusKeranjang(keranjang.id)"
                ></b-icon>
              </td>
            </tr>
            <tr>
              <td colspan="6" align="right">
                <strong>Total Harga : </strong>
              </td>
              <td align="right">
                <strong>Rp. {{ totalHarga }}</strong>
              </td>
              <td></td>
            </tr>
          </tbody>
        </table>
        <div class="container row d-flex justify-content-end">
          <form class="col-md-4 mt-3" v-on:submit.prevent>
            <div class="form-group">
              <label for="nama">Nama :</label>
              <input
                class="form-control"
                type="text"
                id="nama"
                v-model="pesan.nama"
              />
            </div>
            <div class="form-group">
              <label for="no-meja">No meja :</label>
              <select class="form-control" id="no-meja" v-model="pesan.no_meja">
                <option>1</option>
                <option>2</option>
                <option>3</option>
                <option>4</option>
                <option>5</option>
              </select>
            </div>
            <button type="submit" class="btn btn-danger" @click="bayar">
              <b-icon-cart></b-icon-cart> Bayar
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
      pesan: {},
    };
  },
  methods: {
    setKeranjang(data) {
      this.keranjangs = data;
    },
    hapusKeranjang(id) {
      axios
        .delete("http://localhost:3000/keranjangs/" + id)
        .then(() => {
          this.$toast.success("Berhasil Menghapus Keranjang", {
            // optional options Object
            position: "top-right",
          });

          // update data keranjang
          axios("http://localhost:3000/keranjangs")
            .then((response) => this.setKeranjang(response.data))
            .catch((error) => console.log(error));
        })
        .catch((error) => console.log(error));
    },
    bayar() {
      if (this.pesan.nama && this.pesan.no_meja) {
        this.pesan.keranjangs = this.keranjangs;
        axios
          .post("http://localhost:3000/pesanans", this.pesan)
          .then(() => {
            this.keranjangs.map((keranjang) => {
              return axios
                .delete("http://localhost:3000/keranjangs/" + keranjang.id)
                .catch((error) => console.log(error));
            });

            this.$router.push({ path: "/pesanan-sukses" });
            this.$toast.success("Pesanan sedang dibuat", {
              // optional options Object
              position: "top-right",
            });
          })
          .catch((error) => console.log(error));
      } else {
        this.$toast.error("Isi yang bener Boi !", {
          position: "top-right",
        });
      }
    },
  },
  mounted() {
    axios("http://localhost:3000/keranjangs")
      .then((response) => this.setKeranjang(response.data))
      .catch((error) => console.log(error));
  },
  computed: {
    totalHarga() {
      return this.keranjangs.reduce((item, index) => {
        return item + index.products.harga * index.jumlah_pemesanan;
      }, 0);
    },
  },
};
</script>

<style>
</style>