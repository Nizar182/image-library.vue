<template>
    <div>
      <label for="imageInput" class="custom-file-upload">
        <input id="imageInput" type="file" class="form-control" @change="uploadImage" />
        Upload Gambar
      </label>
      <div v-if="uploadStatus" :class="{ 'alert-success': uploadStatus.includes('berhasil'), 'alert-danger': uploadStatus.includes('error') }" role="alert">
        {{ uploadStatus }}
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        uploadStatus: null,
      };
    },
    methods: {
      async uploadImage(event) {
        const file = event.target.files[0];
  
        if (!file) {
          this.uploadStatus = "Upload error: No file selected.";
          return;
        }
  
        const allowedExtensions = ["jpg", "jpeg", "bmp", "png", "gif"];
        const fileExtension = file.name.split(".").pop().toLowerCase();
  
        if (!allowedExtensions.includes(fileExtension)) {
          this.uploadStatus = "Hanya boleh file gambar (jpg, jpeg, bmp, png, gif).";
          return;
        }
  
        const maxSize = 2 * 1024 * 1024;
        if (file.size > maxSize) {
          this.uploadStatus = "File gambar terlalu besar (max. 2 MB).";
          return;
        }
  
        const formData = new FormData();
        formData.append("image", file);
  
        try {
          const response = await this.$axios.post(
            "https://api.imgbb.com/1/upload?key=a58dbd0e0e14a01606e350d1b95eb5ab",
            formData
          );
  
          const imageUrl = response.data.data.url;
          this.$emit("imageUploaded", imageUrl);
          this.uploadStatus = "Gambar berhasil diupload.";
        } catch (error) {
          console.error("Error uploading image:", error);
          this.uploadStatus = "Upload error.";
        }
      },
    },
  };
  </script>

<style scoped>
.custom-file-upload {
  display: inline-block;
  padding: 10px 15px;
  font-size: 14px;
  font-weight: bold;
  color: #fff;
  background-color: #007bff;
  cursor: pointer;
  border: 1px solid #007bff;
  border-radius: 4px;
  transition: background-color 0.3s ease;
}

.custom-file-upload:hover {
  background-color: #0056b3;
}

.alert-success {
  color: #155724;
  background-color: #d4edda;
  border-color: #c3e6cb;
}

.alert-danger {
  color: #721c24;
  background-color: #f8d7da;
  border-color: #f5c6cb;
}
</style>