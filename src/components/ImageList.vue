<template>
    <div>
      <ul class="list-group">
        <li v-for="(imageUrl, index) in imageUrls" :key="index" class="list-group-item">
          <img :src="imageUrl" alt="Uploaded" class="img-fluid" />
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      imageUrls: Array,
    },
    methods: {
      removeImage(index) {
        this.$emit("removeImage", index);
      },
    },
  };
  </script>
  
  <style scoped>
  .list-group-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  
  .img-fluid {
    max-width: 100%;
    height: auto;
  }
  </style>