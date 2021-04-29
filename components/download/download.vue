<template>
	<view><web-view :webview-styles="webviewStyles" :src="url"></web-view></view>
</template>

<script>

import { mapState } from 'vuex';
export default {
	data() {
		return {
			webviewStyles: {
				progress: {
					color: '#FF3333'
				}
			},
			url: ''
		};
	},
	onShow() {
		this.url = 'http://www.cj713.cn/privacy.html';
	}
};
</script>

<style></style>
