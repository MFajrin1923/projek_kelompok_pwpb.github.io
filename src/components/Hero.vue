<template>
  <div class="hero">
    <!-- Desktop Version -->
    <div class="d-none d-md-block hero-desktop">
      <div class="row my-5">
        <div class="col">
          <div class="d-flex h-100">
            <div class="justifycontent-center align-self-center">
              <h2><strong>Pesen Makanan, </strong>gak perlu ribet antri</h2>
              <p>Ayo buruan pesen makanan favorit lo !</p>
              <router-link to="/foods" class="btn btn-lg btn-danger mt-3"
                ><b-icon-arrow-right></b-icon-arrow-right> Pesan</router-link
              >
            </div>
          </div>
        </div>
        <div class="col">
          <img src="../assets/images/hero.png" alt="" width="110%" />
        </div>
      </div>
    </div>

    <!-- Mobile Version -->
    <div class="d-sm-block d-md-none">
      <div class="row my-5">
        <div class="col-md-6">
          <img src="../assets/images/hero.png" alt="" width="100%" />
        </div>
        <div class="col-md-6 my-4">
          <div class="d-flex h-100 justify-content-center">
            <div class="justifycontent-center align-self-center text-center">
              <h2><strong>Pesen Makanan</strong><br />gak perlu ribet antri</h2>
              <p>Ayo buruan pesen makanan favorit lo !</p>
              <router-link to="/foods" class="btn btn-lg btn-danger"
                ><b-icon-arrow-right></b-icon-arrow-right> Pesan</router-link
              >
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Hero",
};
</script>

<style>
</style>