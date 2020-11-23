<template>
  <div class="scanner">

    <h1>Select QRCode Image to Scan 🔬</h1>
    <div>
      <label class="custom-file-upload" v-show="!cam" @click="useCam">Use Camera</label>
    </div>
    <div class="stream">
      <qrcode-stream v-if="cam" :track="repaint" @detect="onDetect" @decode="onDecode"></qrcode-stream>
    </div>
    <div class="margin"><p class="or">OR</p></div>
    <label for="select-image" :class="[processing? 'processing' : '', 'custom-file-upload']">{{ processing? 'Hang on...' : 'Choose Image' }}</label>
    <qrcode-capture id="select-image" :multiple="false" @detect="onDetect" @decode="onDecode" :capture="false" />
    <div class="value">
      <div v-if="decodedValue" @click="goToSite"> {{ this.decodedValue }} </div>
    </div>
  </div>
</template>

<script>
import './_scanner.scss';
import { Vue, Component } from 'vue-property-decorator';
import { QrcodeStream, QrcodeCapture } from 'vue-qrcode-reader';

@Component({
  components: {
    QrcodeCapture,
    QrcodeStream,
  },
})

export default class Scanner extends Vue {
  decodedValue = '';

  cam = false;

  processing = false;

  useCam() {
    this.cam = !this.cam;
  }

  // eslint-disable-next-line class-methods-use-this
  repaint(location, ctx) {
    const {
      topLeftCorner,
      topRightCorner,
      bottomLeftCorner,
      bottomRightCorner,
    } = location;
    console.log(location);
    // eslint-disable-next-line no-param-reassign
    ctx.strokeStyle = 'blue';

    ctx.beginPath();
    ctx.moveTo(topLeftCorner.x, topLeftCorner.y);
    ctx.lineTo(bottomLeftCorner.x, bottomLeftCorner.y);
    ctx.lineTo(bottomRightCorner.x, bottomRightCorner.y);
    ctx.lineTo(topRightCorner.x, topRightCorner.y);
    ctx.lineTo(topLeftCorner.x, topLeftCorner.y);
    ctx.closePath();

    ctx.stroke();
  }

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
