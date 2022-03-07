<template >
	<view class="uni-flex uni-column">
		<view class="gray_color"></view>
		<view>
			<uni-swipe-action>
				<uni-swipe-action-item v-for="(item, index) in itemList" :show="isOpened"  :left-options="item.leftOption" :right-options="rightList" :key="index" @change="swipeChange($event, index)" @click="swipeClick($event, index)">
					<view class="content-box">
						<text class="content-text">{{ item.userMsg }}</text>
					</view>
				</uni-swipe-action-item>
			</uni-swipe-action>
		</view>

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
				date: getDate({
					format: true
				}),
				result: '',
				cacheKey: 'forgetWhat',
				isOpened: '',
				itemList:[],
				rightList: [{
							text: '编辑'
						},
						{
							text: '删除',
							style: {
								backgroundColor: 'rgb(255,58,49)'
							}
						}
					],
				leftList: [{
							text: '密码xxxxxx'
						},
						{
							text: '复制密码',
							style: {
								backgroundColor: 'rgb(0, 170, 255)'
							}
						}
					]
			}
		},
		onReady() {
			// let   xxxx = [{
			// 	userMsg: 'QQ',
			// 	pwd: 'sdfsads'
			// },{
			// 	userMsg: '平安桌面',
			// 	pwd: 'ASS122#$@DS'
			// }]
			// uni.setStorageSync(this.cacheKey,xxxx);
			
			this.reShow();
			
		},
		methods: {
			reShow(){
				console.log('reshow');
				let localData = uni.getStorageSync(this.cacheKey);
				
				console.log(localData);
				
				if(!localData || localData.length==0){
					localData = [{
						userMsg: '未保存过密码，请尝试【左右滑动】',
						pwd: 'your password',
						leftOption: []
					}]
				}
				
				for(let i in localData){
					localData[i]['leftOption'] = [{
								text: localData[i].pwd
							},
							{
								text: '复制密码',
								style: {
									backgroundColor: 'rgb(0, 170, 255)'
								}
							}];
				}
				this.itemList = localData;
				console.log(this.itemList)
			},
			swipeChange(e, index) {
				console.log('返回：', e);
				console.log('当前索引：', index);
			},
			swipeClick(e, index) {
				console.log('=======')
				console.log(e)
				let {
					content
				} = e;
				if (content.text === '删除') {
					uni.showModal({
						title: '提示',
						content: '是否删除',
						success: res => {
							if (res.confirm) {
								this.itemList.splice(index, 1);
								let localData = uni.getStorageSync(this.cacheKey);
								localData.splice(index,1)
								uni.setStorageSync(this.cacheKey,localData);
								this.reShow();
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					});
				} else if (content.text === '添加') {
					if (this.itemList.length < 10) {
						this.itemList.push({
							options: [{
									text: '置顶'
								},
								{
									text: '标记为已读',
									style: {
										backgroundColor: 'rgb(254,156,1)'
									}
								},
								{
									text: '删除',
									style: {
										backgroundColor: 'rgb(255,58,49)'
									}
								}
							],
							content: '新增'
						});
					} else {
						uni.showToast({
							title: `最多添加十条数据`,
							icon: 'none'
						});
					}
				} else if (content.text === '复制密码') {
					console.log(this.itemList[index].pwd);
					uni.setClipboardData({
						data:this.itemList[index].pwd,
						success:function () {
							console.log('success');
						}
					})
				} else if (content.text === '编辑') {
					let tmp = this.itemList[index];
					uni.reLaunch({
						url: '/pages/tabBar/my/form?userMsg='+tmp.userMsg +"&pwd="+tmp.pwd+"&index="+index,
					});
				}else {
					uni.showToast({
						title: `点击了${e.content.text}按钮`,
						icon: 'none'
					});
				}
			},
			exit(x) {}
		},
		onShow() {}
	}
</script>

<style>
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
	.content-box {
		flex: 1;
		height: 44px;
		line-height: 44px;
		padding: 0 15px;
		position: relative;
		background-color: #fff;
		border-bottom-color: #f5f5f5;
		border-bottom-width: 1px;
		border-bottom-style: solid;
	}
	
	.content-text {
		font-size: 15px;
	}
</style>
