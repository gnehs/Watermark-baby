<template>
  <div class="adjust-container">
    <div class="left">
      <h3>預覽</h3>
      <div class="watermark-preview" :class="{ loading }">
        <img v-if="fileurl" :src="fileurl" />
        <div
          class="watermark-text-preview"
          :class="watermarkStyle"
          :style="{
                        backgroundImage: `url('${watermarkPreview}')`,
                        height: watermarkPreviewTextHeight + 'px',
                        width: watermarkPreviewTextWidth + 'px',
                        transform: `scale(${watermarkPreviewTextScale})`,
                        backgroundPosition: `${watermarkTextPositionx}px ${watermarkTextPositiony}px`,
                    }"
        ></div>
      </div>
    </div>
    <div class="right">
      <h3>調整</h3>
      <div
        class="watermark-text"
        :class="watermarkStyle"
        :style="{
                    color: watermarkTextColor,
                    fontSize: watermarkTextSize + 'px',
                }"
      >
        <pre
          :style="{
                        padding: watermarkTextPadding + 'px',
                        transform: `rotate(${watermarkTextRotate}deg)`,
                    }"
          v-html="watermarkText"
        ></pre>
      </div>
      <v-expansion-panels>
        <v-expansion-panel>
          <v-expansion-panel-header>文字與樣式</v-expansion-panel-header>
          <v-expansion-panel-content>
            <v-color-picker
              dot-size="25"
              hide-canvas
              hide-inputs
              hide-mode-switch
              show-swatches
              swatches-max-height="100"
              v-model="watermarkTextColor"
            />
            <br />
            <v-textarea
              outlined
              label="文字"
              rows="2"
              v-model="watermarkText"
              hide-details
              @change="genWatermark"
            />
            <br />
            <v-slider
              label="大小"
              hide-details
              thumb-label
              v-model="watermarkTextSize"
              max="72"
              min="1"
              @change="genWatermark"
            />
          </v-expansion-panel-content>
        </v-expansion-panel>
        <v-expansion-panel>
          <v-expansion-panel-header>距離與位置</v-expansion-panel-header>
          <v-expansion-panel-content>
            <v-slider
              v-if="watermarkPreviewTextHeight"
              label="垂直位置"
              hide-details
              thumb-label
              v-model="watermarkTextPositiony"
              :max="watermarkPreviewTextHeight"
              min="0"
              @change="genWatermark"
            />
            <v-slider
              v-if="watermarkPreviewTextWidth"
              label="水平位置"
              hide-details
              thumb-label
              v-model="watermarkTextPositionx"
              :max="watermarkPreviewTextWidth"
              min="0"
              @change="genWatermark"
            />
            <v-slider
              label="間距"
              hide-details
              thumb-label
              v-model="watermarkTextPadding"
              max="20"
              min="0"
              @change="genWatermark"
            />
            <v-slider
              label="旋轉"
              hide-details
              thumb-label
              v-model="watermarkTextRotate"
              max="45"
              min="-45"
              @change="genWatermark"
            />
            <v-checkbox
              v-model="watermarkStyle"
              label="重複 - 讓浮水印重複出現"
              value="repeat"
              hide-details
              @change="genWatermark"
            />
          </v-expansion-panel-content>
        </v-expansion-panel>
      </v-expansion-panels>
    </div>
  </div>
</template>
<style lang="sass" scoped>
.adjust-container
    display: flex
    .left,.right
        padding: 4px
        h3
            margin-bottom: 8px
    .left
        width: 50%
        flex: 1
        .watermark-preview
            position: relative
            transition: all .2s ease
            img
                width: 100%
                margin-bottom: -8px
            &.loading
                filter: blur(3px)
            .watermark-text-preview
                width: 100%
                height: 100%
                transform-origin: left top
                position: absolute
                top: 0
                left: 0
                &.repeat
                    background-repeat: repeat

    .right
        width: 50%
        max-width: 300px
        .watermark-text
            background-image: linear-gradient(45deg,#efefef 25%,rgba(239,239,239,0) 25%,rgba(239,239,239,0) 75%,#efefef 75%,#efefef),linear-gradient(45deg,#efefef 25%,rgba(239,239,239,0) 25%,rgba(239,239,239,0) 75%,#efefef 75%,#efefef)
            background-color: #fff
            background-position: 0 0,10px 10px
            background-size: 21px 21px
            background-repeat: repeat
            overflow: hidden
            height: 100px
            position: relative
            pre
                display: inline-block
                font-family: 'Lato', 'Noto Sans TC', sans-serif
@media screen and(max-width: 512px)
    .adjust-container
        flex-direction: column
        .left,.right
            width: 100%
            max-width: 100%
        .left
            .watermark-preview
                display: inline-block
                img
                    max-height: 200px
                    width: auto
</style>
<script>
import html2canvas from "html2canvas";
export default {
  name: "style-adjust",
  data: () => ({
    watermarkStyle: [],
    watermarkText: "僅供〇〇〇申辦使用",
    watermarkTextColor: "#000C",
    watermarkTextSize: 16,
    watermarkTextPadding: 4,
    watermarkTextRotate: 0,
    watermarkTextPositiony: 0,
    watermarkTextPositionx: 0,
    watermarkPreview: null,
    watermarkPreviewTextScale: null,
    watermarkPreviewTextWidth: null,
    watermarkPreviewTextHeight: null,
    watermarkResult: null,
    loading: false
  }),
  props: ["fileurl"],
  created() {
    this.genWatermark();
  },
  watch: {
    watermarkTextColor() {
      this.genWatermark();
    }
  },
  methods: {
    genWatermark() {
      this.$nextTick(() => {
        this.loading = true;
        // get scale size
        let img = document.querySelector(".watermark-preview img");
        let scaleSize = 1;
        if (img.naturalWidth) {
          scaleSize = img.naturalWidth / img.clientWidth;
          this.watermarkPreviewTextScale = img.clientWidth / img.naturalWidth;
          this.watermarkPreviewTextWidth = img.naturalWidth;
          this.watermarkPreviewTextHeight = img.naturalHeight;
        }
        //gen preview text
        html2canvas(
          document.querySelector(".adjust-container .watermark-text pre"),
          {
            backgroundColor: null,
            scale: scaleSize,
            imageTimeout: 0
          }
        ).then(canvas =>
          canvas.toBlob(blob => {
            this.watermarkPreview = URL.createObjectURL(blob);
            this.genNaturalSizeImg();
          })
        );
      });
    },
    genNaturalSizeImg() {
      this.$nextTick(() => {
        // get scale size
        let img = document.querySelector(".watermark-preview img");
        let scaleSize = 1;
        if (img.naturalWidth) {
          scaleSize = img.naturalWidth / img.clientWidth;
        }
        // gen natural Size image
        html2canvas(document.querySelector(".watermark-preview"), {
          useCORS: true,
          scale: scaleSize,
          imageTimeout: 0
        }).then(canvas =>
          canvas.toBlob(blob => {
            let res = URL.createObjectURL(blob);
            this.watermarkResult = res;
            this.$emit("handleDownloadUrl", res);
            this.loading = false;
          })
        );
      });
    }
  }
};
</script>
