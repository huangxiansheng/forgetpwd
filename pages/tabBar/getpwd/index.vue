<template>
	<view class="uni-flex uni-column">
		<view class="gray_color"></view>

		<form @submit="bindSave">
			<view class="uni-flex uni-row form-row">
				<view class="uni-flex form-lable">
					<text class="uni-bold">使用字符</text>
					<text class="color-red uni-bold" style="margin-top: 4rpx;">*</text>
				</view>
				<view class="uni-flex form-input">

					<view class="uni-list">
						<checkbox-group @change="checkboxChange">
							<label class="uni-list-cell uni-list-cell-pd" v-for="item in items" :key="item.value">
								<view>{{item.name}}</view>
								<view>
									<checkbox :value="item.value" :checked="item.checked" />
								</view>
							</label>
						</checkbox-group>
					</view>

				</view>
			</view>

			<view class="uni-flex uni-row form-row">
				<view class="uni-flex form-lable">
					<text class="uni-bold">密码长度</text>
					<text class="color-red uni-bold" style="margin-top: 4rpx;">*</text>
				</view>
				<view class="uni-flex form-input">
					<picker style="width:100%" @change="pwdlChange" name='idType' :value="pwdIndex" :range="pwdlength"
						range-key="name">
						<view class="uni-input">{{pwdlength[pwdIndex].name}}</view>
					</picker>
				</view>
			</view>

			<view>
				<button class="primary saveButton" style="margin-top: 20rpx;" form-type="submit">生成</button>
			</view>
		</form>

		<view class="gray_color"></view>

		<view class="uni-flex uni-row form-row">
			<view class="uni-flex form-lable">
				<text class="uni-bold">密码</text>
			</view>
			<view class="uni-flex form-input">
				<input class="uni-input" :value="result" />
			</view>
		</view>
		<view>
			<button class="primary saveButton" style="margin-top: 20rpx;" @click="toForm">存储</button>
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
				title: '生成密码',
				date: getDate({
					format: true
				}),
				pwdlength: [{
					name: '6'
				}, {
					name: '7'
				}, {
					name: '8'
				}, {
					name: '9'
				}, {
					name: '10'
				}, {
					name: '11'
				}, {
					name: '12'
				}],
				pwdIndex: 2,
				items: [{
						value: "1",
						name: "小写字母a-z",
						checked: true
					},
					{
						value: "2",
						name: "大写A-Z",
						checked: true
					},
					{
						value: "3",
						name: "数字0-9",
						checked: true
					},
					{
						value: "4",
						name: "特殊字符",
						checked: true
					},
				],
				itemsValue : ['1','2','3','4'],
				idIndex: 0,
				showFlag: false,
				result: ''
			}
		},
		methods: {
			toForm: function() {
				uni.reLaunch({
					url: '/pages/tabBar/my/form?userMsg=&pwd='+this.result+"&index=-1",
				});
			},
			pwdlChange: function(e) {
				this.pwdIndex = e.detail.value;
			},
			checkboxChange: function(e) {
				var items = this.items;
					this.itemsValue = e.detail.value;
				for (var i = 0, lenI = items.length; i < lenI; ++i) {
					const item = items[i]
					if (this.itemsValue.indexOf(item.value) >= 0) {
						this.$set(item, 'checked', true)
					} else {
						this.$set(item, 'checked', false)
					}
				}
				console.log(this.itemsValue);
			},
			bindSave: function(e) {
				var length = this.pwdlength[this.pwdIndex].name;
				var string = "abcdefghijklmnopqrstuvwxyz";//小写
				var strUpper = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";//大写
				var numeric = '0123456789';
				var punctuation = '!@#$%&*';//特殊字符
				var password = "";
				var r = [];//把  itemsValue 复制给r
				if(this.itemsValue.indexOf("1")>=0){r.push("1")};
				if(this.itemsValue.indexOf("2")>=0){r.push("2")};
				if(this.itemsValue.indexOf("3")>=0){r.push("3")};
				if(this.itemsValue.indexOf("4")>=0){r.push("4")};
				
				while (password.length < length) {
					var entity1 = Math.ceil(string.length * Math.random() * Math.random()) - 1;
					var entity2 = Math.ceil(strUpper.length * Math.random() * Math.random()) - 1;
					var entity3 = Math.ceil(numeric.length * Math.random() * Math.random()) - 1;
					var entity4 = Math.ceil(punctuation.length * Math.random() * Math.random()) - 1;
					
					var entity5 = Math.ceil(r.length * Math.random() * Math.random()) - 1;
					if(entity5>=0){
						if (r[entity5] =='1' && password.length < length) {
							password += string.charAt(entity1);
						}
						if (r[entity5] =='2' && password.length < length) {
							password += strUpper.charAt(entity2);
						}
						if (r[entity5] =='3' && password.length < length) {
							password += numeric.charAt(entity3);
						}
						if (r[entity5] =='4' && password.length < length) {
							password += punctuation.charAt(entity4);
						}
					}else{
						break
					}
				}
				if (password.trim()) {
					console.log(!password.match(RegExp("["+punctuation+"]")))
					if (this.itemsValue.indexOf("1")>=0 && !password.match(RegExp("["+string+"]"))) {
						console.log(password+"====1");
						this.bindSave();
					}else if (this.itemsValue.indexOf("2")>=0 && !password.match(RegExp("["+strUpper+"]"))) {
						console.log(password+"====2");
						this.bindSave();
					}else if (this.itemsValue.indexOf("3")>=0 && !password.match(RegExp("["+numeric+"]"))) {
						console.log(password+"====3");
						this.bindSave();
					}else if (this.itemsValue.indexOf("4")>=0 && !password.match(RegExp("["+punctuation+"]"))) {
						console.log(password+"====4");
						this.bindSave();
					}else{
						this.result = password;
						console.log(password);
					}
				} else {
					console.log(password);
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
</style>
