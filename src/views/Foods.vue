<template>
  <div class="home">
    <Navbar />
    <div class="container my-4">
      <div class="row my-5">
        <div class="col">
          <div class="d-flex h-100">
            <div class="justifycontent-center align-self-center">
              <h4>
                <strong class="bg-black text-white p-2 px-3 radius-left"
                  >50% Off </strong
                ><span class="bg-gradient p-2 px-3">murid SMKN 11</span>
              </h4>
              <h1 class="mt-4">
                Pesan Makanan <br />
                tanpa harus mengantri
              </h1>
              <h5 class="mt-3">
                Hey! Our Delicious food is waiting for you, we are <br />
                always ready to serve you with delicious food.
              </h5>
            </div>
          </div>
        </div>
        <div class="col">
          <img src="../assets/images/chef.png" alt="" width="110%" />
        </div>
      </div>
      <div class="row">
        <div class="col-12 text-center">
          <h1><span class="border-nav">Our Menus</span></h1>
        </div>
        <div class="col d-flex justify-content-around my-5">
          <div
            class="card cursor-pointer"
            style="width: 18rem"
            @click="setSnacks()"
            :class="isActive === 1 ? 'badge-danger' : 'badge-info'"
          >
            <div class="card-body text-center">
              <img
                class="rounded-circle bg-white p-1"
                src="https://img.icons8.com/wired/100/000000/tapas.png"
              />
              <h1 class="card-title">Snacks</h1>
            </div>
          </div>
          <div
            class="card"
            style="width: 18rem"
            @click="setFoods()"
            :class="isActive === 2 ? 'badge-danger' : 'badge-info'"
          >
            <div class="card-body text-center cursor-pointer">
              <img
                class="rounded-circle bg-white p-1"
                src="https://img.icons8.com/wired/100/000000/rice-bowl.png"
              />
              <h1 class="card-title">Foods</h1>
            </div>
          </div>
          <div
            class="card"
            style="width: 18rem"
            @click="setDrinks()"
            :class="isActive === 3 ? 'badge-danger' : 'badge-info'"
          >
            <div class="card-body text-center cursor-pointer">
              <img
                class="rounded-circle bg-white p-1"
                src="https://img.icons8.com/wired/100/000000/orange-soda.png"
              />
              <h1 class="card-title">Drinks</h1>
            </div>
          </div>
        </div>
      </div>
      <div class="row mt-3">
        <div class="col-8">
          <h2>
            <span class="border-nav">Daftar</span> <strong>Makanan</strong>
          </h2>
        </div>
        <div class="col-4">
          <div class="input-group my-3">
            <input
              v-model="search"
              type="text"
              class="form-control"
              placeholder="Cari Makanan Favoritmu ..."
              aria-label="Cari"
              aria-describedby="basic-addon1"
              @keyup="searchFoods"
            />
            <span class="input-group-text ml-1 badge-danger" id="basic-addon1"
              ><b-icon-search></b-icon-search
            ></span>
          </div>
        </div>
      </div>

      <div class="row my-3">
        <div
          class="col-md-4 my-4"
          v-for="product in products"
          :key="product.id"
        >
          <CardProduct :product="product" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Navbar from "@/components/Navbar.vue";
import CardProduct from "@/components/CardProduct.vue";
import axios from "axios";

export default {
  name: "Foods",
  components: {
    Navbar,
    CardProduct,
  },
  data() {
    return {
      products: [],
      search: "",
      isActive: 0,
      setSearch: "",
    };
  },
  methods: {
    setProduct(data) {
      this.products = data;
    },
    searchFoods() {
      axios(`http://localhost:3000/${this.setSearch}?q=` + this.search)
        .then((response) => this.setProduct(response.data))
        .catch((error) => console.log(error));
    },
    setSnacks() {
      axios("http://localhost:3000/snacks")
        .then((response) => {
          this.setProduct(response.data);
          this.isActive = 1;
          this.setSearch = "snacks";
        })
        .catch((error) => console.log(error));
    },
    setFoods() {
      axios("http://localhost:3000/foods")
        .then((response) => {
          this.setProduct(response.data);
          this.isActive = 2;
          this.setSearch = "foods";
        })
        .catch((error) => console.log(error));
    },
    setDrinks() {
      axios("http://localhost:3000/drinks")
        .then((response) => {
          this.setProduct(response.data);
          this.isActive = 3;
          this.setSearch = "drinks";
        })
        .catch((error) => console.log(error));
    },
    handleActive(id) {
      this.isActive = id;
    },
  },
  mounted() {
    axios("http://localhost:3000/products")
      .then((response) => this.setProduct(response.data))
      .catch((error) => console.log(error));
  },
};
</script>

<style>
</style>