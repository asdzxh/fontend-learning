<template>
	<view :style="{height: barHeight}" class="uni-status-bar">
		<slot></slot>
	</view>
</template>

<script setup>
	import { ref } from 'vue'
	let statusBarHeight = uni.getSystemInfoSync().statusBarHeight + "px"
	const barHeight = ref(statusBarHeight)
</script>

<style scoped>
	.uni-status-bar {
		position: fixed;
		width: 750rpx;
		height: 20px;
		background-color: var(--light);
	}
</style>