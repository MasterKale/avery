<template>
  <div class="avery">
    <div id="controls">
      <h1>Label Generator</h1>
      <hr>
      <h2>Labels</h2>
      <section>
        <p>
          Place each label on a separate line. It should be fine to copy-paste from Excel...
        </p>
        <p>
          <textarea v-model="barcodeInput" cols="30" rows="25"></textarea>
        </p>
      </section>
      <hr>
      <h2>Press Ctrl+P when you're ready to print these labels!</h2>
      <section>
        <p>This app was mainly tested in <strong>Chrome</strong>! It'll probably work in Firefox too...</p>
        <p>When printing, try to use the same page margins as the <strong>Avery 5167 label template</strong> (clockwise from top):</p>
        <ul>
          <li><strong>Top:</strong> 0.5"</li>
          <li><strong>Right:</strong> 0.31"</li>
          <li><strong>Bottom:</strong> 0.42"</li>
          <li><strong>Left:</strong> 0.39"</li>
        </ul>
      </section>
      <hr>
      <h2>Controls</h2>
      <section>
        <p>
          If you need to, change the values below to tweak the label layout until they're suitable for printing:
        </p>
        <p>
          <label for="height">Label Height (default: <span v-once>{{ settings.height }}px</span>)</label>
          <input id="height" name="height" type="number" step="0.01" v-model.number="settings.height">
        </p>
        <p>
          <label for="width">Label Width (default: <span v-once>{{ settings.width }}px</span>)</label>
          <input id="width" name="width" type="number" step="0.01" v-model.number="settings.width">
        </p>
        <p>
          <label for="padding">Internal Label Padding (default: <span v-once>{{ settings.padding }}px</span>)</label>
          <input id="padding" name="padding" type="number" step="0.01" v-model.number="settings.padding">
        </p>
        <p>
          <label for="cornerRadius">Label Corner Radius (default: <span v-once>{{ settings.cornerRadius }}px</span>)</label>
          <input id="cornerRadius" name="cornerRadius" type="number" min="5" step="0.01" v-model.number="settings.cornerRadius">
        </p>
        <p>
          <label for="rightMargin">Gap Between Labels (default: <span v-once>{{ settings.rightMargin }}px</span>)</label>
          <input id="rightMargin" name="rightMargin" type="number" step="0.01" v-model.number="settings.rightMargin">
        </p>
        <p>
          <label for="barcodeHeightMM">Barcode Height (default: <span v-once>{{ settings.barcodeHeightMM }}mm</span>)</label>
          <input id="barcodeHeightMM" name="barcodeHeightMM" type="number" min="5" step="0.1" v-model.number="settings.barcodeHeightMM">
        </p>
      </section>
      <hr>
    </div>
    <Label
      v-for="(code, index) in barcodes"
      :key="index"
      :text="code"
      :height="settings.height"
      :width="settings.width"
      :padding="settings.padding"
      :rightMargin="settings.rightMargin"
      :barcodeHeightMM="settings.barcodeHeightMM"
      :cornerRadius="settings.cornerRadius"
    />
  </div>
</template>

<script>
import Label from './Label';

export default {
  name: 'Avery',
  components: {
    Label,
  },
  data() {
    return {
      barcodeInput: '',
      barcodes: [],
      settings: {
        height: 48,
        width: 166,
        padding: 12,
        rightMargin: 20,
        barcodeHeightMM: 5,
        cornerRadius: 5,
      }
    };
  },
  watch: {
    barcodeInput: function(newVal) {
      if (newVal) {
        this.barcodes = newVal.split('\n');
      } else {
        this.barcodes = [];
      }
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
#controls {
  padding: 1em;
}

p, hr {
  margin: 1em 0;
}

ul {
  margin: 1em;
}

label {
  font-weight: bold;
  margin-right: 0.5em;
}

input {
  display: block;
  font-size: 1em;
}

@media print {
  #controls {
    display: none;
  }
}
</style>
