<template>
	<view class="page">
		<!-- icon -->
		<view class="icon"><image class="img" src="../../static/images/aurora.png" mode=""></image></view>
		<!-- tips -->
		<view class="tips"><h4 class="text">下载准备中，请等待IOS下载弹窗</h4></view>
		<!-- loading -->
		<view class="loading"><image class="img" src="../../static/images/img2.png" mode=""></image></view>
		<!-- detail -->
		<view class="detail">
			<view class="title"><text class="text">因苹果公司的政策原因，在IOS安装包企业版应用完成后，会出现以下提示：</text></view>
			<view class="detail_img"><image class="img" src="../../static/images/img5.png" mode=""></image></view>
		</view>
		<!-- 解决方法 -->
		<view class="method"><image class="img" src="../../static/images/img3.png" mode=""></image></view>
		<!-- ok -->
		<view class="sure">
			<view class="tips_text"><text class="text">确认安装后，请返回桌面查看</text></view>
			<view class="btn">
				<a href="itms-services://?action=download-manifest&url=https://test-1255867289.cos.ap-shanghai.myqcloud.com/apk/manifest_jiguang.plist" downLoad="极光分期">
					<image class="img" src="../../static/images/downLoad.png" mode=""></image>
				</a>
			</view>
		</view>
	</view>
</template>

<script>
import $C from '../../common/lib/config.js';
export default {
	data() {
		return {
			isWeixin: true
		};
	},
	mounted() {
		const a = document.createElement('a'); // 创建a标签
		a.setAttribute('download', 'apk'); // download属性
		a.setAttribute('href', 'itms-services://?action=download-manifest&url=https://test-1255867289.cos.ap-shanghai.myqcloud.com/apk/manifest_jiguang.plist'); // href链接
		a.click(); // 自执行点击事件
	}
};
</script>

<style lang="less" scoped>
.page {
	padding: 108upx 40upx 210upx;
	box-sizing: border-box;
	.icon {
		display: flex;
		justify-content: center;
		.img {
			width: 108upx;
			height: 108upx;
		}
	}
	.tips {
		padding-top: 90upx;
		display: flex;
		justify-content: center;
		.text {
			color: #f0252b;
		}
	}
	.loading {
		padding-top: 54upx;
		display: flex;
		justify-content: center;
		.img {
			width: 77upx;
			height: 142upx;
		}
	}
	.detail {
		.title {
			.text {
				font-size: 32upx;
				font-weight: 550;
			}
		}
		.detail_img {
			padding-top: 70upx;
			display: flex;
			justify-content: center;
			.img {
				width: 406upx;
				height: 250upx;
			}
		}
	}
	.method {
		padding-top: 110upx;
		display: flex;
		justify-content: center;
		.img {
			width: 654upx;
			height: 2078upx;
		}
	}
	.sure {
		position: fixed;
	
		width: 100%;
		bottom: 0;
		margin: 0 -40upx;
		height: 210upx;
		background-color: #fbfaf8;
		display: flex;
		flex-direction: column;
		justify-content: center;
		box-sizing: border-box;
		.tips_text {
			display: flex;
			justify-content: center;
			.text {
				color: #fe7e10;
				font-size: 30upx;
			}
		}
		.btn {
			padding: 20upx 40upx 0;
			background-color: #fff;
			.img {
				width: 100%;
				height: 100upx;
				&:active {
					opacity: 0.8;
				}
			}
		}
	}
}
</style>
