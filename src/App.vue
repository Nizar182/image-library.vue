<template>
  <div id="app">
    <header class="text-center my-4">
      <h1>Image Gallery App</h1>
    </header>
    <div class="container">
      <ImageUploader @imageUploaded="addImageUrl" />
      <ImageList :imageUrls="imageUrls" />
    </div>
  </div>
</template>

<script>
import ImageUploader from "@/components/ImageUploader.vue";
import ImageList from "@/components/ImageList.vue";

export default {
  components: {
    ImageUploader,
    ImageList,
  },
  data() {
    return {
      imageUrls: [],
    };
  },
  methods: {
    addImageUrl(imageUrl) {
      this.imageUrls.push(imageUrl);
      this.saveToLocalStorage();
    },
    saveToLocalStorage() {
      localStorage.setItem("imageUrls", JSON.stringify(this.imageUrls));
    },
  },
  mounted() {
    const storedImageUrls = localStorage.getItem("imageUrls");
    if (storedImageUrls) {
      this.imageUrls = JSON.parse(storedImageUrls);
    }
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.container {
  max-width: 600px;
  margin: 0 auto;
}
</style>