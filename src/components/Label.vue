<template>
  <div class="label" :style="labelStyle">
    <span v-html="labelText" :style="spanStyle"></span>
    <img :src="generatedImage" :alt="text">
  </div>
</template>

<script>
import bwipjs from 'bwip-js';

export default {
  name: 'Label',
  props: {
    text: String,
    height: {
      type: Number,
      default: 75,
    },
    width: {
      type: Number,
      default: 262.5,
    },
    padding: {
      type: Number,
      default: 2,
    },
    rightMargin: {
      type: Number,
      default: 10,
    },
    barcodeHeightMM: {
      type: Number,
      default: 9.5
    },
    cornerRadius: {
      type: Number,
      default: 5,
    }
  },
  data() {
    return {
      canvas: null,
      generatedImage: null,
      options: {
        bcid: 'datamatrixrectangular',
        scale: 2,
        includetext: false,
      }
    };
  },
  mounted() {
    this.canvas = document.createElement('canvas');
    // Render a barcode if text has been passed in
    if (this.text) {
      this.generateBarcode(this.text);
    }
  },
  watch: {
    labelCodeRaw: 'generateBarcode',
    barcodeHeightMM: function() {
      this.generateBarcode(this.labelCodeRaw);
    },
  },
  methods: {
    generateBarcode(newVal) {
      if (newVal) {
        const options = {
          ...this.options,
          text: newVal,
          height: this.barcodeHeightMM,
        };

        bwipjs(
          this.canvas,
          options,
          (err, cvs) => {
            if (err) {
              // console.error('error generating barcode:', err);
            } else {
              this.generatedImage = cvs.toDataURL();
            }
          }
        );
      }
    }
  },
  computed: {
    labelParts: function() {
      return Array.from(this.text.split('|'));
    },
    labelCodeRaw: function() {
      return this.labelParts[0];
    },
    labelText: function() {
      const parts = this.labelParts;
      parts.splice(0, 1);
      return parts.join('<br>');
    },
    labelStyle: function() {
      return {
        height: `${this.height}px`,
        width: `${this.width}px`,
        padding: `${this.padding}px`,
        'border-radius': `${this.cornerRadius}px`,
        'margin-right': `${this.rightMargin}px`,
        'text-align': 'center',
      };
    },
    spanStyle: function() {
      return {
      }
    },
  }
}
</script>

<style lang="scss" scoped>
.label {
  display: inline-block;
  position: relative;
  vertical-align: middle;
  border: 1px solid black;
  border-radius: 5px;
}

span {
  display: inline-block;
}

@media print {
  .label {
    border: 0;
  }
}
</style>
