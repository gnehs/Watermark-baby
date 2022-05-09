<template>
    <v-row justify="center" align="center">
        <v-col cols="10" sm="8" md="6" lg="4">
            <div class="index-container">
                <video width="256" height="256" autoplay loop muted>
                    <source src="logo.mp4" type="video/mp4">
                </video>
                <h1>浮水印寶寶</h1>
                <p>協助您為重要證件圖片加上浮水印，防止遭他人盜用。</p>
                <nuxt-link class="wr-btn" to="/generator">開始使用</nuxt-link>
                <nuxt-link class="wr-btn non-bg" to="/about">了解更多</nuxt-link>
            </div>
        </v-col>
    </v-row>
</template>
<style lang="sass" scoped>
.index-container
	text-align: center
</style>
<script>
export default {};
</script>
