<template>
  <div class="image">
    <span class="helper"></span>
    <img :src="url" alt="25">
  </div>
</template>

<script>

export default {
  name: "Picture.vue",
  props: {
    url: {
      type: String,
      required: true
    }
  }
}
</script>

<style scoped>
.image {
  text-align: center;
  width: 100%;
  height: 250px;
  white-space: nowrap;
  z-index: 10;
}

.helper {
  display: inline-block;
  height: 100%;
  vertical-align: middle;
}

img {
  vertical-align: middle;
  width: 300px;
  height: 300px;
}

</style>
