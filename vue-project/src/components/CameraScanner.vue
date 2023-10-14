<template>
  <v-card>
    <v-card-text>
        <StreamBarcodeReader
          class="camera"
          @decode="onDecode"
        ></StreamBarcodeReader>
    </v-card-text>
  </v-card>
</template>

<script>


import { StreamBarcodeReader } from "vue-barcode-reader";
export default {
  data() {
    return {
      scanning: false,
      errorC: false, // Define your error condition
    };
  },
  methods: {
    onDecode(decodedData) {
      // Handle the decoded data here
      console.log("Decoded Data:", decodedData);
      this.$emit("barcodeScanned",decodedData)
    },
  },components: {
    StreamBarcodeReader,
  },
};
</script>
<style scoped>
.camera {
  /*height: 20vh; /* Set the height to 30% of the viewport height */
  height: 20vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>