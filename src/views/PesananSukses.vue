<template>
  <div class="pesanan-sukses text-center">
    <Navbar />
    <div class="text-center d-none d-md-block">
      <img src="../assets/images/masak.png" alt="Masak" width="42%" />
    </div>
    <div class="text-center d-sm-block d-md-none">
      <img src="../assets/images/masak.png" alt="Masak" width="100%" />
    </div>
    <h1>Pesanan sedang di proses</h1>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";

export default {
  name: "Pesan",
  components: {
    Navbar,
  },
};
</script>