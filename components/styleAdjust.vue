<template>
	<div class="adjust-container">
		<div class="left">
			<h3>預覽</h3>
			<div
				class="watermark-text"
				:class="watermarkStyle"
				:style="{
					color: watermarkTextColor,
					fontSize: watermarkTextSize + 'px',
				}"
			>
				<span> {{ watermarkText }}</span>
			</div>
			<div class="watermark-preview">
				<img v-if="fileurl" :src="fileurl" />
				<div
					class="watermark-text-preview"
					:class="watermarkStyle"
					:style="{ backgroundImage: `url('${watermarkPreview}')` }"
				></div>
			</div>
			<img v-if="watermarkResult" :src="watermarkResult" width="100%" />
		</div>
		<div class="right">
			<h3>調整</h3>
			<v-color-picker
				dot-size="25"
				hide-canvas
				hide-inputs
				hide-mode-switch
				show-swatches
				swatches-max-height="100"
				v-model="watermarkTextColor"
			></v-color-picker>
			<br />
			<v-textarea
				outlined
				label="文字"
				rows="2"
				v-model="watermarkText"
				hide-details
			/>
			<br />
			<v-text-field
				label="大小"
				type="number"
				outlined
				hide-details
				v-model="watermarkTextSize"
			/>
			<v-checkbox
				v-model="watermarkStyle"
				label="模糊 - 模糊處理，使其無法輕易被去除"
				value="blur"
				hide-details
			></v-checkbox>
			<v-checkbox
				v-model="watermarkStyle"
				label="重複 - 讓浮水印重複出現"
				value="repeat"
				hide-details
			></v-checkbox>
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
        .watermark-text
            text-align: center
            background-image: linear-gradient(45deg,#efefef 25%,rgba(239,239,239,0) 25%,rgba(239,239,239,0) 75%,#efefef 75%,#efefef),linear-gradient(45deg,#efefef 25%,rgba(239,239,239,0) 25%,rgba(239,239,239,0) 75%,#efefef 75%,#efefef)
            background-color: #fff
            background-position: 0 0,10px 10px
            background-size: 21px 21px
            background-repeat: repeat
            span
                padding: 4px
            &.blur
                filter: blur(.5px)
        .watermark-preview
            border-radius: 8px
            position: relative
            img
                width: 100%
            .watermark-text-preview
                width: 100%
                height: 100%
                transform-origin: left top
                position: absolute
                top: 0
                left: 0
                &.repeat
                    background-repeat: repeat
                &.blur
                    filter: blur(.5px)

    .right
        width: 50%
        max-width: 300px
</style>
<script>
import html2canvas from "html2canvas";
export default {
	name: "style-adjust",
	data: () => ({
		watermarkStyle: [],
		watermarkText: "安安你好，幾歲？住哪？",
		watermarkTextColor: "#000C",
		watermarkTextSize: 16,
		watermarkPreview: null,
		watermarkResult: null,
	}),
	props: ["fileurl"],
	watch: {
		watermarkStyle: {
			handler() {
				this.$nextTick(() => {
					this.genWatermark();
				});
			},
		},
		watermarkText: {
			handler() {
				this.$nextTick(() => {
					this.genWatermark();
				});
			},
		},
		watermarkTextColor: {
			handler() {
				this.$nextTick(() => {
					this.genWatermark();
				});
			},
		},
		watermarkTextSize: {
			handler() {
				this.$nextTick(() => {
					this.genWatermark();
				});
			},
		},
	},
	destroyed() {},
	created() {
		this.$nextTick(() => {
			this.genWatermark();
		});
	},
	methods: {
		genWatermark() {
			console.log("gen");
			html2canvas(
				document.querySelector(".adjust-container .watermark-text span"),
				{
					backgroundColor: null,
					logging: false,
				}
			).then((canvas) =>
				canvas.toBlob((blob) => {
					this.watermarkPreview = URL.createObjectURL(blob);
				})
			);
			this.genImage();
		},
		genImage() {
			// get scale size
			let img = document.querySelector(".watermark-preview img"),
				scaleSize = 1;
			if (img.naturalWidth) {
				scaleSize = img.naturalWidth / img.clientWidth;
			}
			html2canvas(document.querySelector(".adjust-container"), {
				logging: false,
				scale: scaleSize,
			}).then((canvas) =>
				canvas.toBlob((blob) => {
					let res = URL.createObjectURL(blob);
					this.watermarkResult = res;
					this.$emit("handleDownloadUrl", res);
				})
			);
		},
	},
};
</script>
