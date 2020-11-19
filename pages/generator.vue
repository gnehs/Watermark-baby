<template>
	<v-row justify="center" align="center">
		<v-col cols="10" sm="8" md="6" lg="4">
			<transition name="fade" mode="out-in">
				<div v-if="step == 0" key="import_image">
					<h1>匯入圖片</h1>
					<p>
						Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec
						semper dui a pharetra lobortis.
					</p>
					<file-upload @handleFileURL="handleFileURL" />
					<v-btn
						rounded
						color="primary"
						style="min-width: 150px"
						@click="step = 1"
						:disabled="!fileurl"
					>
						下一步
					</v-btn>
				</div>
				<div v-else-if="step == 1" key="style_adjust">
					<h1>調整樣式</h1>
					<p>
						Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec
						semper dui a pharetra lobortis.
					</p>
					<style-adjust
						:fileurl="fileurl"
						v-if="fileurl"
						@handleDownloadUrl="handleDownloadUrl"
					/>
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
						Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec
						semper dui a pharetra lobortis.
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
		},
		handleDownloadUrl(url) {
			this.downloadurl = url;
		},
	},
};
</script>
