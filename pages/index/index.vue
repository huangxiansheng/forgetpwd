<template>
	<view class="content" >
		<image class="logo" src="/static/logo.png" v-on:click="bindLogin"></image>
		<view class="text-area">
			<text class="title">{{title}}</text>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: '忘记密码'
			}
		},
		onLoad() {

		},
		methods: {
			bindLogin() {
				console.log(998);
				uni.reLaunch({
					url: '/pages/tabBar/getpwd/index',
				});
				console.log(997);
				
			}
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
