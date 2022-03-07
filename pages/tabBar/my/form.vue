<template>
	<view class="uni-flex uni-column">
		<view class="gray_color"></view>

		<form @submit="bindSave">
			<view class="uni-flex uni-row form-row">
				<view class="uni-flex form-lable">
					<text class="uni-bold">标签</text>
				</view>
				<view class="uni-flex form-input">
					<input class="uni-input" placeholder="请输入标签" :value="userMsg" @input="msgInput"/>
					<text class="uni-icon" v-if="showClearIcon" @click="clearIcon">&#xe434;</text>
				</view>
			</view>
			
			<view class="uni-flex uni-row form-row">
				<view class="uni-flex form-lable">
					<text class="uni-bold">密码</text>
				</view>
				<view class="uni-flex form-input">
					<input class="uni-input" placeholder="请输入密码" :value="pwd" @input="pwdInput" :password="false" />
				</view>
			</view>
			
			<view>
				<button class="primary saveButton" style="margin-top: 20rpx;" form-type="submit">保存</button>
			</view>
		</form>
	</view>
</template>

<script>
	import uniDrawer from "@/components/uni-drawer/uni-drawer.vue" //抽屉
	import uniIcons from "@/components/uni-icons/uni-icons.vue" //图标
	import tTable from '@/components/t-table/t-table.vue';
	import tTh from '@/components/t-table/t-th.vue';
	import tTr from '@/components/t-table/t-tr.vue';
	import tTd from '@/components/t-table/t-td.vue';

	export default {
		components: {
			uniDrawer,
			uniIcons,
			tTable,
			tTh,
			tTr,
			tTd
		},
		data() {
			return {
				title: '生成密码',
				showPassword: true,
				cacheKey: 'forgetWhat',
				userMsg: '请输入标签',
				showClearIcon: true,
				pwd: 'your password',
				index: -1
			}
		},
        onLoad(options) {
			console.log(options)
			
			if(options.userMsg){
				this.userMsg = options.userMsg;
			}
			if(options.pwd){
				this.pwd = options.pwd;
			}
			if(options.index){
				this.index = options.index;
			}
        },
		methods: {
			bindSave: function(e) {
				var localData = uni.getStorageSync(this.cacheKey);
				var realData = JSON.parse(JSON.stringify(localData)); 
				console.log(realData);
				var addData = {
						userMsg: this.userMsg,
						pwd: this.pwd
					};
					console.log(11);
					
				if(realData){
					console.log(33);
					if(this.index == -1){
						console.log(44);
						realData.push(addData)
					}else{
						realData[this.index] = addData;
					}
				}else{
					realData = [addData];
				}
				console.log(22);
				
				
				uni.setStorageSync(this.cacheKey,realData);
				uni.reLaunch({
					url: '/pages/tabBar/my/index',
				});
			},
			clearIcon: function() {
			    this.userMsg = '';
			    this.showClearIcon = false;
			},
			msgInput: function(event) {
			    this.userMsg = event.detail.value;
			    if (event.detail.value.length > 0) {
			        this.showClearIcon = true;
			    } else {
			        this.showClearIcon = false;
			    }
			},
			pwdInput: function(event) {
			    this.pwd = event.detail.value;
			},
			exit(x) {}
		},
		onShow() {}
	}
</script>

<style>
	.uni-icon {
		padding-top: 16rpx;
	}
	.click-bg {
		color: rgb(248, 248, 248);
	}

	.form-row {
		margin-left: 32rpx;
		margin-top: 16rpx;
		margin-bottom: 16rpx;
	}

	.form-lable {
		padding-top: 16rpx;
		width: 250rpx;
	}

	.form-input {
		width: 500rpx;
	}

	.mgl-32 {
		margin-left: 32rpx;
	}

	.pdl-32 {
		padding-left: 32rpx;
	}

	.gray_color {
		background-color: #f8f8f8;
		height: 20rpx;
	}

	.color-red {
		color: #F3605D;
	}

	.title_img {
		padding-right: 8rpx;
		height: 24rpx;
		width: 24rpx;
	}

	.title_ {
		background-color: #ffffff;
		height: 32rpx;
		font-size: 28rpx;
		line-height: 32rpx;
	}

	.title {
		background-color: #ffffff;
		height: 64rpx;
		font-size: 32rpx;
		font-weight: 600;
		line-height: 100rpx;
		margin-left: 32rpx;
	}

	.saveButton {
		background-color: #3A63E0;
		color: #ffffff;
		width: 80%;
		margin-top: 100rpx;
	}
</style>
