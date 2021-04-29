<template>
	<view>
		<view class="example-body message">
			<button class="button popup__success" @click="popupDialogOpen()"><text class="button-text">激活弹出层</text></button>
		</view>

		<!-- 对话框 -->
		<uni-popup ref="popupDialog" type="dialog" @change="change">
			<view class="dialog">
				<view class="modal">
					<view class="header"><text @click="popupDialogClose()" class="cancel">x</text></view>
					<view class="title"><text>请确认信息</text></view>
					<view class="phoneBox">
						<text>188 1234 1234</text>
						<view @click="popupDialogClose">x</view>
					</view>
					<view class="phoneInput">
						<input class="input" type="text" value="" placeholder="请输入短信验证码" placeholder-class="peacText" />
						<text v-if="is_send" :style="sendColor" class="reset">{{ `重新获取(${time})` }}</text>
						<text v-else @click="sendCode()" :style="sendColor" class="reset">发送验证码</text>
					</view>
					<view class="forget">收不到验证码？</view>
					<view class="btnBox"><button class="btn">免费领额度</button></view>
				</view>
			</view>
		</uni-popup>
	</view>
</template>

<script>
import uniPopupMessage from '@/components/uni-popup/uni-popup-message.vue';
import uniPopupDialog from '@/components/uni-popup/uni-popup-dialog.vue';
import uniPopupShare from '@/components/uni-popup/uni-popup-share.vue';
export default {
	components: {
		uniPopupMessage,
		uniPopupDialog,
		uniPopupShare
	},
	data() {
		return {
			time: 5,
			is_send: false //是否发送验证码  true 已发送  fasle 未发送
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
	methods: {
		/**
		 * 打开弹窗
		 * @param {Object} type
		 */
		popupDialogOpen() {
			this.$refs.popupDialog.open();
		},
		popupDialogClose() {
			console.log(11);
			this.$refs.popupDialog.close();
		},
		/**
		 * 发送验证码
		 */
		sendCode() {
			setInterval(() => {
				if (this.time > 0) {
					this.is_send = true;
					this.time--;
				} else {
					this.is_send = false;
				}
			}, 1000);

			this.is_send = !this.is_send;
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
.dialog {
	width: 600upx;
	display: flex;
	justify-content: center;
}
.modal {
	height: 600upx;
	width: 100%;
	background-color: #fff;
	padding: 40upx;
	border-radius: 10px;
	box-sizing: border-box;
	.title,
	.peacText {
		display: flex;
		justify-content: center;
		font-size: 32upx;
		color: #666;
	}
	.phoneBox {
		margin-top: 20upx;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.phoneInput {
		margin-top: 60upx;
		display: flex;
		justify-content: space-between;
		align-items: center;
		border: 2px solid #e5e5e5;
		padding: 10upx;
		font-size: 24upx;
		.input {
			flex: 1;
			height: 80upx;
		}

		.reset {
			font-size: 28upx;
			padding-left: 10upx;
			display: flex;
			align-items: center;
			&::before {
				display: inline-block;
				content: ' ';
				width: 2px;
				height: 60upx;
				background-color: #e5e5e5;
				margin-right: 10upx;
				font-size: 24upx;
			}
		}
	}
	.forget {
		margin: 20upx 0 15upx;
		display: flex;
		justify-content: flex-end;
		color: #0056b3;
		font-size: 28upx;
	}
	.btnBox {
		.btn {
			background-color: #ff9b36;
			color: #fff;
			width: 90%;
			height: 80upx;
			line-height: 80upx;
			&:active {
				opacity: 0.8;
			}
		}
	}
}
</style>
