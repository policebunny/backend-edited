<template>
  <v-card :height="h-75">
    <v-card-title class="headline">Scanned Barcodes 1</v-card-title>
    <v-divider></v-divider>
    <v-card-text  :max-height="50">
      <v-virtual-scroll
      
      :items ="scannedBarcodes"
      height="175"
      item-height="29"
      >
      <template v-slot:default="{ item }">
          <v-list-item :title="`${item.data}`" :subtitle="`${item.timestamp}`">
            <template v-slot:append>
            <v-btn icon="mdi-content-copy" size="x-small" variant="tonal" @click=" copyBarcode(item) "></v-btn>  

            <v-btn icon="mdi-delete-outline" size="x-small" variant="tonal" @click=" removeBarcode(item) "></v-btn>
          </template>
         
          </v-list-item>
      </template>

      </v-virtual-scroll>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  props: {
    scannedBarcodes: [],
  },
  methods: {
    removeBarcode(index) {
      // Remove the barcode at the specified index
      //this.scannedBarcodes.splice(this.scannedBarcodes.findIndex(e =>e.timestamp === index.timestamp),1)
      this.$emit("removebarcode", index)
    },
    copyBarcode(barcode) {
      // Copy the barcode data to the clipboard
      navigator.clipboard.writeText(barcode.data);
    },
  },
};
</script>

<style>
  .scrollable-container {
  max-height: 300px; /* Set the fixed height for the container */
  overflow-y: auto; /* Enable scrolling if content exceeds container height */
}

</style>