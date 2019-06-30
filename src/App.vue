<template>
  <div id="app">
    <button v-on:click="recognize">recognize</button>
    <img id="text-img" alt="Vue logo" src="./assets/testocr.png">

    <div>
      <div class="file-upload-form">
        Upload an image file:
        <input type="file" @change="previewImage" accept="image/*">
      </div>
      <div class="image-preview" v-if="imageData.length > 0">
        <img id="loadImg" class="preview" :src="imageData">
      </div>
    </div>
  </div>
</template>

<script>
import Tesseract from 'tesseract.js';
import { TesseractWorker } from 'tesseract.js';
const worker = new TesseractWorker();
const consola = require('consola');

export default {
  name: 'app',
  data: () => {
    return {
      imageData: ""
    }
  },
  methods: {
    recognize() {
      // const img = document.getElementById('text-img');
      const img = document.getElementById('loadImg');
      consola.log('---16---');
      worker.recognize(img)
              .progress(progress => {
                consola.log('progress', progress);
              }).then(result => {
        consola.log('result', result);
        const m = result.text.match(/[0-9]+keal/)[0];
        const hoge = m.replace('keal', 'kcal');
        consola.log('---hoge---');
        consola.log(hoge);
        consola.log('---hoge---');

      });
    },
    previewImage: function(event) {
      // Reference to the DOM input element
      var input = event.target;
      // Ensure that you have a file before attempting to read it
      if (input.files && input.files[0]) {
        // create a new FileReader to read this image and convert to base64 format
        var reader = new FileReader();
        // Define a callback function to run, when FileReader finishes its job
        reader.onload = (e) => {
          // Note: arrow function used here, so that "this.imageData" refers to the imageData of Vue component
          // Read image as base64 and set to imageData
          this.imageData = e.target.result;
        }
        // Start the reader job - read file as a data url (base64 format)
        reader.readAsDataURL(input.files[0]);
      }
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>



