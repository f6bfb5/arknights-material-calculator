<template>
  <div class="row g-0">
    <div class="col-4 m-0 p-2 d-flex justify-content-center align-items-center">
      <img
        :src="imgUrl"
        class="img-fluid"
        :alt="cn"
      />
    </div>
    <div class="col-8 m-0 p-2">
      <h4>{{ jp }}</h4>
      <div class="d-flex">
        <span
          class="input-group-text"
          id="own"
        >持有</span>
        <input
          type="text"
          class="form-control"
          aria-label=""
          aria-describedby=""
          :value="own"
          @input="$emit('input-own', $event.target.value)"
        />
      </div>
      <div class="d-flex">
        <span
          class="input-group-text"
          id="need"
        >需要</span>
        <input
          type="text"
          class="form-control"
          aria-label=""
          aria-describedby=""
          :value="need"
          @input="$emit('input-need', $event.target.value)"
        />
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    props: ["imgName", "cn", "jp", "own", "need"],
    computed: {
      imgUrl: function() {
        let images = require.context("../assets/", false, /\.png$/);
        return images("./" + this.imgName + ".png");
      }
    }
  };
</script>

<style>
</style>