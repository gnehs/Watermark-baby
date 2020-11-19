<template>
	<div
		class="upload-container"
		:class="{ fileuploaded: fileurl, isOnDrop }"
		@dragover="onDrop(true)"
		@drop="onDrop(false)"
		@dragleave="onDrop(false)"
	>
		<div class="inner-container" v-if="!filename || isOnDrop">
			<span v-if="!isOnDrop">點擊此處匯入圖片或將圖片拖移至此</span>
			<span v-else>將圖片拖移至此放開</span>
		</div>
		<div class="inner-container" v-else>
			<img v-if="fileurl" :src="fileurl" /><br />
			<span>{{ filename }}</span>
		</div>
		<input type="file" @change="getFiles($event)" />
	</div>
</template>
<style lang="sass" scoped>
.upload-container
    width: 100%
    text-align: center
    height: 150px
    border-width: 3px
    border-style: dashed
    border-color: #CCC
    position: relative
    border-radius: 8px
    margin: 8px 0
    transition: all .25s ease
    &.fileuploaded
        border-color: #DDD
    &.isOnDrop
        transform: scale(1.1)
        border-color: #666
        box-shadow: inset 0 1px 0 rgba(255,255,255,.3), 0 22px 70px 4px rgba(0,0,0,0.23), 0 0 0 1px rgba(0, 0, 0, 0.0)
    .inner-container
        position: absolute
        margin: auto
        top: 0
        bottom: 0
        left: 0
        right: 0
        pointer-events: none
        color: #A300EB
        padding: 10px
        height: max-content
        img
            max-width: 200px
            max-height: 100px
            border-radius: 8px
            box-shadow: inset 0 1px 0 rgba(255,255,255,.3), 0 22px 70px 4px rgba(0,0,0,0.23), 0 0 0 1px rgba(0, 0, 0, 0.0)
    input
        z-index: 1
        height: 100%
        width: 100%
        opacity: 0
</style>
<script>
export default {
	name: "file-upload",
	data: () => ({
		filename: null,
		fileurl: null,
		isOnDrop: false,
	}),
	destroyed() {},
	created() {},
	methods: {
		getFiles(e) {
			this.onDrop(false);
			let file = e.target.files[0];
			let filename = file.name;
			if (filename.endsWith(".jpg") || filename.endsWith(".png")) {
				this.filename = filename;
				this.fileurl = URL.createObjectURL(file);
				this.$emit("handleFileURL", this.fileurl);
			} else {
				alert("不支援的檔案");
			}
		},
		onDrop(val) {
			this.isOnDrop = val;
		},
	},
};
</script>
