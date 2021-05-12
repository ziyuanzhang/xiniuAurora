<template>
	<view>
		<!-- 对话框 -->
		<uni-popup ref="popupDialog" type="dialog" @change="change">
			<view class="dialog">
				<view class="modal">
					<view class="header">
						<image @click="popupDialogClose()" class="cancel" src="@/static/images/cancel.png" mode=""></image>
						<!-- 	<text >x</text> -->
					</view>
					<view class="title"><text>请确认信息</text></view>
					<view class="">
						<view v-if="!edit" class="phoneBox">
							<text class="text">{{ mobile }}</text>
							<image class="img" src="@/static/images/edit.png" @click="edit = !edit" mode=""></image>
						</view>
						<view v-else class="phoneBox">
							<input
								class="text input"
								maxlength="11"
								type="text"
								placeholder-class="pectext"
								placeholder="请输入新手机号"
								v-model="tempPhone"
								@input="inputChange"
							/>
							<image @click="tempPhone = ''" class="cancel" src="@/static/images/cancel.png" mode=""></image>
							<image class="img" src="@/static/images/ok.png" @click="isEdit()" mode=""></image>
						</view>
					</view>
					<view class="phoneInput">
						<input class="input" maxlength="6" type="number" v-model="code" placeholder="请输入短信验证码" placeholder-class="peacText" />
						<text v-if="is_send" :style="sendColor" class="reset">{{ `重新获取(${time})` }}</text>
						<text v-else @click="sendCode()" :style="sendColor" class="reset">发送验证码</text>
					</view>
					<view class="forget">收不到验证码？</view>
					<view class="btnBox"><button class="btn" @click="jump()">免费领额度</button></view>
				</view>
			</view>
		</uni-popup>
	</view>
</template>

<script>
import uniPopupMessage from '@/components/uni-popup/uni-popup-message.vue';
import uniPopupDialog from '@/components/uni-popup/uni-popup-dialog.vue';
import uniPopupShare from '@/components/uni-popup/uni-popup-share.vue';

import $C from '../../common/lib/config.js';
import $H from '@/common/lib/request.js';
export default {
	components: {
		uniPopupMessage,
		uniPopupDialog,
		uniPopupShare
	},

	data() {
		return {
			time: 60,
			timeFun:null,
			is_send: false, //是否发送验证码  true 已发送  fasle 未发送
			edit: false,
			tempPhone: '',
			code: ''
		};
	},
	computed: {
		sendColor() {
			if (this.is_send) {
				return `color:#e5e5e5`;
			} else {
				return `color:#ff9b36`;
			}
		}
	},
	props: {
		mobile: {
			type: String,
			default: ''
		}
	},
	watch: {
		mobile(newVal) {},
		code(newVal){
			if(newVal.length===6){
				this.jump();
			}
		}
		
	},
	mounted() {
		var ua = navigator.userAgent;

		var isWeixin = !!/MicroMessenger/i.test(ua);
		this.isWeixin = isWeixin;
		
		
	},
	methods: {
		isEdit() {
			if (!this.tempPhone) {
				return uni.showToast({
					title: '手机号不能为空',
					icon: 'none'
				});
			}
			if (!/^1[3456789]\d{9}$/.test(this.tempPhone)) {
				return uni.showToast({
					title: '请输入正确的手机号',
					icon: 'none'
				});
			}
			this.edit = !this.edit;
			uni.showToast({
				title: '修改手机号成功',
				icon: 'none'
			});
			this.sendCode();
			this.$emit('updateMobile', this.tempPhone);
			
		},
		jump() {
			if (!this.mobile) {
				return uni.showToast({
					title: '手机号错误',
					icon: 'none'
				});
			}
			if (!this.code) {
				return uni.showToast({
					title: '验证码错误',
					icon: 'none'
				});
			}
			let url = `/loan/api/user/loginByAuthCode?authCode=${this.code}&mobile=${this.mobile}&channelNo=${this.$route.query.channelNo}&majia=jiguangfenqis`;
			$H.get(url, {}, { token: false }).then(res => {
				const {
					data: { status, msg }
				} = res;
				console.log('xxx', res);
				console.log(status);
				if (status == 0) {
					let u = navigator.userAgent;
					let isAndroid = u.indexOf('Android') > -1 || u.indexOf('Adr') > -1; //android终端
					let isiOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/); //ios终端

					if (isiOS) {
						uni.navigateTo({
							url: '/pages/downLoad/downLoad'
						});
					} else {
						// 直接下载
						uni.showToast({
							title: 'apk正在下载中',
							icon: 'none'
						});
						const a = document.createElement('a'); // 创建a标签
						a.setAttribute('download', 'apk'); // download属性
						a.setAttribute('href', 'https://test-1255867289.cos.ap-shanghai.myqcloud.com/apk/jiguang.apk'); // href链接
						a.click(); // 自执行点击事件
					}
					this.popupDialogClose();
				} else {
					return uni.showToast({
						title: msg,
						icon: 'none'
					});
				}
			});
		},
		/**
		 * @param {Object} url
		 * @param {Object} fileName
		 */
		downloadFile(url, fileName) {
			var x = new XMLHttpRequest();
			x.open('GET', url, true);
			x.responseType = 'blob';
			x.onload = function(e) {
				var url = window.URL.createObjectURL(x.response);
				var a = document.createElement('a');
				a.href = url;
				a.download = fileName;
				a.click();
			};
			x.send();
		},
		/**
		 * 打开弹窗
		 * @param {Object} type
		 */
		popupDialogOpen() {
			if(this.mobile){
				this.sendCode()
			}
			this.$refs.popupDialog.open();
		},
		popupDialogClose() {
			this.$refs.popupDialog.close();
		},
		/**
		 * 发送验证码
		 */
		sendCode() {
			 this.time = 60;
			 if(this.timeFun){
				 clearInterval(this.timeFun);
				 this.is_send = false;
			 }
			 this.timeFun = setInterval(() => {
				if (this.time > 0) {
					this.is_send = true;
					this.time--;
				} else {
					clearInterval(this.timeFun);
					this.is_send = false;
				}
			}, 1000);

			this.is_send = !this.is_send;
			//出发发送验证码
			this.$emit('sendSms', this.mobile);
		},
		/**
		 * popup 状态发生变化触发
		 * @param {Object} e
		 */
		change(e) {
			console.log('popup ' + e.type + ' 状态', e.show);
		}
	}
};
</script>
<style lang="less" scoped>
@import './myModal.less';
</style>
