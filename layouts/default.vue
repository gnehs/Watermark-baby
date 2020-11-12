<template>
	<v-app>
		<v-app-bar color="white" flat>
			<v-toolbar-title color="primary">浮水印寶寶</v-toolbar-title>

			<v-spacer></v-spacer>

			<v-btn text color="primary" to="/"> 首頁 </v-btn>
			<v-btn text color="primary" to="/generator"> 產生器 </v-btn>
			<v-btn text color="primary" to="/about"> 關於 </v-btn>
		</v-app-bar>

		<transition name="fade" mode="out-in">
			<v-main :key="$route.path">
				<router-view />
			</v-main>
		</transition>
	</v-app>
</template>
<script>
import Vue from "vue";
export default {
	name: "App",
	data: () => ({}),
	destroyed() {},
	created() {},
	methods: {},
};
</script>