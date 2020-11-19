<template>
	<v-row justify="center" align="center">
		<v-col cols="10" sm="8" md="6" lg="4">
			<transition name="fade" mode="out-in">
				<div v-if="step == 0" key="import_image">
					<h1>匯入圖片</h1>
					<p>於此匯入圖片開始製作浮水印吧！</p>
					<file-upload @handleFileURL="handleFileURL" />
				</div>
				<div v-else-if="step == 1" key="style_adjust">
					<h1>調整樣式</h1>
					<p>在這裡調整浮水印的樣式</p>
					<style-adjust
						:fileurl="fileurl"
						v-if="fileurl"
						@handleDownloadUrl="handleDownloadUrl"
					/>
					<v-btn text rounded color="primary" @click="step = 0"> 返回 </v-btn>
					<v-btn
						rounded
						color="primary"
						style="min-width: 150px"
						@click="step = 2"
					>
						完成
					</v-btn>
				</div>
				<div v-else key="finish">
					<h1>完成</h1>
					<p>
						圖片產生完成！輕觸底下下載來取得圖片，如果您使用
						iOS，請長按圖片來下載。
					</p>
					<img v-if="downloadurl" :src="downloadurl" width="100%" />
					<a :href="downloadurl" download="s.png"> dl</a
					><v-btn
						rounded
						color="primary"
						style="min-width: 150px"
						:to="downloadurl"
						target="_blank"
						download="s.png"
					>
						下載圖片
					</v-btn>
					<v-btn
						text
						rounded
						color="primary"
						style="min-width: 150px"
						@click="step = 0"
					>
						繼續製作
					</v-btn>
				</div>
			</transition>
		</v-col>
	</v-row>
</template>

<script>
export default {
	data: () => ({
		// 0:import_image --> 1:style_adjust --> 2:finish
		step: 0,
		fileurl: null,
		downloadurl: null,
	}),
	destroyed() {},
	created() {},
	methods: {
		handleFileURL(url) {
			this.fileurl = url;
			this.step = 1;
		},
		handleDownloadUrl(url) {
			this.downloadurl = url;
		},
	},
};
</script>
