<template>
  <div class="home">
    <div class="row">
      <div class="col-8">
        <b-form-file
              v-model="file"
              accept="image/*"
              :state="Boolean(file)"
              placeholder="Choose a file..."
              drop-placeholder="Drop file here..."
              @change="onFileChange"
            ></b-form-file>
      </div>
      <div class="col-4">
        <b-button variant="primary" @click="detectObject">判定！</b-button>
      </div>
    </div>

    <div class="row">
      <div v-if="image">
        <img :src="image.src" style="width:100%"/>
      </div>   
    </div>
 
    <div class="row">
      <div v-if="resultPrediction">
        {{resultPrediction}}}
      </div>    
    </div>

  </div>
</template>

<script>
import * as cocoSsd from '@tensorflow-models/coco-ssd';
export default {
  name: 'home',
  data(){
    return {
      file: null,
      image: null,
      resultPrediction: null
    }
  },

  methods:{
    detectObject(){
     // Load the model.
      cocoSsd.load().then(model => {
        // detect objects in the image.
        model.detect(this.image).then(predictions => {
          console.log('Predictions: ', predictions);
          this.resultPrediction = predictions
        });
      });
    },
    onFileChange: function(e){
      this.resultPrediction = null
      var files = e.target.files || e.dataTransfer.files;
      if (!files.length) {
          return;
      }
      this.createImage(files[0]);
    },
    createImage(file) {
      this.image = new Image();
      var reader = new FileReader();
      reader.onload = function(e) {
          this.image.src = e.target.result;
      }.bind(this);
      reader.readAsDataURL(file);
    }
  }
}
</script>
