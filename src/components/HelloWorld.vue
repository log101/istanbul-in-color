<script setup>
import { extractColors } from 'extract-colors'
import { ref, watch } from 'vue'

defineProps({
  msg: String,
});

const colorList = ref([])

function displayFile(input, ev) {
  var file = input.target.files[0];

  if (file) {
    var reader = new FileReader();
    var image = document.getElementById('uploadedFile');

    reader.onload = function (e) {
      image.src = e.target.result;

      extractColors(e.target.result)
        .then(arr => {
          // start with the area covering a bigger portion of the image
          colorList.value = arr.sort((a, b) => a.area < b.area)
        })
        .catch(console.error)
    };
    reader.readAsDataURL(file);
  }
}
</script>

<template>
  <div className="p-4 flex flex-col gap-4">
    <h1 className="text-3xl font-bold underline">{{ msg }}</h1>

    <input id="uploadInput" type="file" @change="displayFile" />

    <!-- Container to display the uploaded file -->
    <div id="fileContainer">
      <img id="uploadedFile" style="max-width: 300px; max-height: 300px" />
    </div>

    <ul class="flex flex-wrap gap-2 justify-left">
      <li className="leading-[0]" v-for="item in colorList">
      <span className="block border border-black border-opacity-20 h-6 rounded-xl w-6" :style="{ backgroundColor: item.hex }"></span>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
