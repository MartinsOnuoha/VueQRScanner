<template>
  <div class="scanner">
    <h1>Select QRCode Image to Scan 🔬</h1>
    <label for="select-image" :class="[processing? 'processing' : '', 'custom-file-upload']">{{ processing? 'Hang on...' : 'Choose Image' }}</label>
    <qrcode-capture id="select-image" :multiple="false" @detect="onDetect" @decode="onDecode" />

    <div class="value">
      <div v-if="decodedValue" @click="goToSite"> {{ this.decodedValue }} </div>
    </div>
  </div>
</template>

<script>
import './_scanner.scss';
import { Vue, Component } from 'vue-property-decorator';
import { QrcodeDropZone, QrcodeCapture } from 'vue-qrcode-reader';

@Component({
  components: {
    QrcodeDropZone,
    QrcodeCapture,
  },
})

export default class Scanner extends Vue {
  decodedValue = '';

  processing = false;

  onDecode(decodedString) {
    if (this.isUrl(decodedString)) {
      this.decodedValue = decodedString;
      if (!this.decodedValue.includes('http')) {
        this.decodedValue = `http://${this.decodedValue}`;
      }
      return true;
    }
    return false;
  }

  async onDetect(promise) {
    this.processing = true;
    await promise;
    this.processing = false;
  }

  goToSite() {
    window.open(this.decodedValue, '_blank');
  }

  isUrl = (url) => {
    const regexp = /https?:\/\/w{0,3}\w*?\.(\w*?\.)?\w{2,3}\S*|www\.(\w*?\.)?\w*?\.\w{2,3}\S*|(\w*?\.)?\w*?\.\w{2,3}[\/\?]\S*/;
    return regexp.test(url);
  }
}
</script>
