<template>
  <div>
   <image-uploader @input="loadmodel"/>

    <div class="predict-controls">
      <button class="element button--green" v-on:click="predict" >Predict</button>
      <p>{{predicteddata
        }} </p>
    </div>
  </div>
</template>

<script>
import Component from 'vue-class-component'
import Vue from 'vue'
import * as tf from '@tensorflow/tfjs'
import ImageUploader from '~/components/ImageUploader.vue'

@Component({
  components: {
    ImageUploader
  }
})
export default class TensorFlowExample extends Vue {
  
    model

    imagesrc

    predicteddata ='test'
 previewImage
    mounted() {
      
    }

    async loadmodel(data)
    {
 this.model =  await tf.loadLayersModel('/models/model.json');
 this.imagesrc =data
  let reader = new FileReader
  reader.readAsDataURL(this.imagesrc)
          reader.onload = e => {
            this.previewImage = e.target.result
    }
  }

    async predict() {
      const im = new Image()
im.src = this.previewImage;
const a = tf.browser.fromPixels(im, 3)
  const resized_image = tf.image.resizeBilinear(a, [300,300]).toFloat();
  const batchedImage = resized_image.expandDims(0);
      const prediction = this.model.predict(batchedImage);

      

      prediction.print();

   if(prediction.shape[0] >0)
     this.predicteddata ="NOLLD"
   else
   this.predicteddata ="LLD"
  }
}

</script>

<style>
.field, .field-label {
  height: 30px;
  padding: 0px 15px;
  float: left;
  width: 50%;
}

.field {
  border-radius: 0px 5px 5px 0px;
  border: 1px solid #eee;
  margin-bottom: 15px;
  height: 40px;
}

.col-sm-1:after {
    content: "";
    display: table;
    clear: both;
}

.section, .field-label {
  text-align: left;
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  font-weight: 100;
}

.field-label {
  font-weight: 700;
}

.button-add-example {
  width: 100%;
  margin-bottom: 10px;
}

.button-train {
  width: 100%;
}

.predict-controls {
  padding-top: 30px;
  padding-bottom: 30px;
}

.predict-controls .element {
  width: 50%;
  display: block;
}

button {
  margin-top: 10px;
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  font-weight: 700;
}

</style>
