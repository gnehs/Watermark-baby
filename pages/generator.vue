<template>
  <v-row justify="center" align="center">
    <v-col cols="10" sm="8" md="7" lg="6">
      <transition name="fade" mode="out-in">
        <div v-if="step == 0" key="import_image">
          <h1>匯入圖片</h1>
          <p>於此匯入圖片開始製作浮水印吧！</p>
          <file-upload @handleFileURL="handleFileURL" />
        </div>
        <div v-else-if="step == 1" key="style_adjust">
          <a
            class="wr-btn non-bg"
            @click="step = 0"
            style="
                            text-align: left;
                            margin-bottom: 8px;
                            min-width: auto;
                        "
          >⬅返回</a>
          <h1>調整樣式</h1>
          <p>在這裡調整浮水印的樣式</p>
          <style-adjust :fileurl="fileurl" v-if="fileurl" @handleDownloadUrl="handleDownloadUrl" />
          <div style="text-align: center">
            <a class="wr-btn" @click="step = 2">完成</a>
          </div>
        </div>
        <div v-else key="finish">
          <h1>完成</h1>
          <p>
            圖片產生完成！輕觸底下下載來取得圖片，如果您使用
            iOS，請長按圖片來下載。
          </p>
          <img v-if="downloadurl" :src="downloadurl" width="100%" />
          <div style="text-align: center">
            <a class="wr-btn" :href="downloadurl" :download="`浮水印寶寶_${Date.now()}.png`">下載圖片</a>
            <a class="wr-btn non-bg" @click="step = 0">繼續製作</a>
          </div>
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
    downloadurl: null
  }),
  methods: {
    handleFileURL(url) {
      this.fileurl = url;
      this.step = 1;
    },
    handleDownloadUrl(url) {
      this.downloadurl = url;
    }
  }
};
</script>
