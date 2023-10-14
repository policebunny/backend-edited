<template>
  <v-app :height="h-screen">
    <v-main > 
        <v-layout justify-center>
          <v-flex xs12 md6>
            <!-- Camera Scanner Component -->
            <camera-scanner @barcodeScanned="addBarcode"></camera-scanner>

            <!-- Barcode List Component -->
            <barcode-list :scannedBarcodes="scannedBarcodes" @removebarcode="removeEntry"></barcode-list>
          </v-flex>
        </v-layout>
    </v-main>
  </v-app>
</template>

<script>
import CameraScanner from "./components/CameraScanner.vue";
import BarcodeList from "./components/BarcodeList.vue";
import axios from "axios";
export default {
  data() {
    return {
      baseUrl: "http://localhost:8080",
      //baseUrl: "backende-test-scanner.azurewebsites.net",
      scannedBarcodes: [],
    };
  },
  methods: {
    addBarcode(barcode) {

        this.addEntry({data: barcode, timestamp: new Date().toJSON()});
      //this.scannedBarcodes.push({data: barcode, timestamp: new Date().toJSON()});
      console.log(this.scannedBarcodes);
    },
    addEntry: function(e) {
      axios
        .post(`${this.baseUrl}/barcode`, {
          data: e.data,
          timestamp: e.timestamp
        })
        .then(response => {
          this.scannedBarcodes = response.data;
        });
    },
    editEntry: function(e) {
      axios
        .put(`${this.baseUrl}/barcode/` + e.index, {
          data: e,
          timestamp: new Date().toJSON()
        })
        .then(response => {
          this.scannedBarcodes = response.data; //TODO: change this, do not return full list
        });
    },
    removeEntry: function(e) {
      axios.delete(`${this.baseUrl}/barcode/` + e.index).then(response => {
        this.scannedBarcodes = response.data;
      });
    }
  },
  mounted() {
    axios.get(`${this.baseUrl}/barcode`).then(response => {
      this.scannedBarcodes = response.data;
      console.log(this.scannedBarcodes);
    });
    
  },
  components: {
    CameraScanner,
    BarcodeList,
  },
};
</script>