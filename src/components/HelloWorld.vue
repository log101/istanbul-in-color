<script setup>
defineProps({
  msg: String,
});

function displayFile(input, ev) {
  var file = input.target.files[0];

  if (file) {
    var reader = new FileReader();
    var image = document.getElementById('uploadedFile');

    reader.onload = function (e) {
      image.src = e.target.result;
    };
    reader.readAsDataURL(file);
  }
}
</script>

<template>
  <h1>{{ msg }}</h1>

  <input id="uploadInput" type="file" @change="displayFile" />

  <!-- Container to display the uploaded file -->
  <div id="fileContainer">
    <img id="uploadedFile" style="max-width: 300px; max-height: 300px" />
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
