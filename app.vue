<template>
  <div id="app">
    <h2>Barcode Scanner in Nuxt.js</h2>
    <div v-if="!initialized">Initializing...</div>
    <button v-if="initialized" @click="toggleScanning">{{ scanning ? "Stop Scanning":"Start Scanning" }}</button>
    <div class="container">
      <ClientOnly fallback-tag="span" fallback="Loading barcode scanner...">
        <BarcodeScanner ref="scanner" @initialized="onInitialized" @scanned="onScanned"></BarcodeScanner>
      </ClientOnly>
    </div>
    <div>
      <div>
        Results:
      </div>
      <ol>
        <li v-for="(barcode,index) in scannedBarcodes" :key="'barcode-'+index">{{ barcode.formatString + ": " + barcode.text }}</li>
      </ol>
    </div>
  </div>
</template>
<script setup lang="ts">
import type { BarcodeResultItem } from 'dynamsoft-barcode-reader-bundle';
import BarcodeScanner from './components/BarcodeScanner.vue';
const scanner = ref();
const initialized = ref(false);
const scanning = ref(false);
const scannedBarcodes = ref<BarcodeResultItem[]>([])
const onInitialized = () => {
  initialized.value = true;
}

const onScanned = (barcodes:BarcodeResultItem[]) => {
  if (barcodes.length>0) {
    scannedBarcodes.value = barcodes;
  }
}

const toggleScanning = () => {
  scanning.value = !scanning.value;
  if (scanner.value) {
    if (scanning.value) {
      scanner.value.start();
    }else{
      scanner.value.stop();
    }
  }
}

</script>

<style lang="css" scoped>
.container {
  position: relative;
  width: 360px;
  height: 360px;
}
</style>