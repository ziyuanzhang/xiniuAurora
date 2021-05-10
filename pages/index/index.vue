<template>
	<view class="page">
		<view v-if="!isWeixin">
			<myModal @sendSms="sendSms" @updateMobile="updateMobile" :mobile="mobile" ref="mymodal"></myModal>
			<view class="logo">
				<image class="img" src="@/static/images/xunfeilogo.jpg" mode=""></image>
			</view>
			<!-- limit 额度 -->
			<view class="limit">
				<view class="zero">0</view>
				<view class="five">
					5
					<text>万</text>
				</view>
				<view class="ten">
					10
					<text>万</text>
				</view>
				<view class="fifteen">
					15
					<text>万</text>
				</view>
				<view class="twenty">
					20
					<text>万</text>
				</view>
				<image class="img" src="@/static/images/limit.png" mode=""></image>
			</view>
			<!-- tick 勾选文字 -->
			<view class="tick">
				<image class="img" src="@/static/images/tick.png" mode=""></image>
			</view>
			<!-- apply 申请三步走-->
			<view class="apply">
				<image src="@/static/images/apply.png" mode=""></image>
			</view>
			<!-- phoneNum 手机号 -->
			<view class="phoneNum">
				<image class="img" src="@/static/images/phone.png" mode=""></image>
				<input v-model="mobile" maxlength="11" type="number" class="ipt" placeholder="输入您的手机号码" />
			</view>
			<!-- code 短信验证码 -->

			<!-- 测测按钮 -->
			<view class="cece">
				<myAnimateButton @click="showModal"><text>立即拿钱</text></myAnimateButton>
			</view>
			<!-- 协议 -->
			<view class="protocol">
				<view class="check">
					<image @click="selected = !selected" v-if="selected" class="img" src="@/static/images/set.png" mode=""></image>
					<image @click="selected = !selected" v-else class="img" src="@/static/images/unset.png" mode=""></image>
				</view>
				<view class="desc">
					<text class="agree">点击"立即拿钱"按钮即同意</text>
					<text class="policy" @click="policy('reg')">《注册协议》</text>
					<text class="policy" @click="policy('yinsi')">《用户隐私协议》</text>
				</view>
			</view>
			<view class="info">
				<!-- 留言 -->
				<view class="message">
					<view class="title`">
						<text class="line">丨</text>
						<text class="title">听听他们怎么说？--来自极光分期用户的心声<strong></strong></text>
					</view>

					<view class="liuyan" v-if="index < 3" v-for="(item, index) in comments" :key="index">
						<view class="avatar">
							<image class="img" :src="item.img" mode=""></image>
						</view>
						<view class="list">
							<view class="top">
								<text class="phone">{{ item.mobile }}</text>
								<text class="state">已申请</text>
							</view>
							<view class="footer">{{ item.desc }}</view>
						</view>
					</view>
				</view>
			</view>
			<!-- 理由 -->
			<view class="reason">
				<!-- 顶部标题 -->
				<view class="reason_top">
					<image class="imgL" src="@/static/images/arrows.png" mode=""></image>
					<text class="text">选择极光分期的理由</text>
					<image class="imgR" src="@/static/images/arrows.png" mode=""></image>
				</view>
				<!-- 底部图片 -->
				<view class="reason_footer">
					<image class="img" src="@/static/images/edugao.png" mode=""></image>
					<image class="img" src="@/static/images/shenhekuai.png" mode=""></image>
					<image class="img" src="@/static/images/lixidi.png" mode=""></image>
					<image class="img" src="@/static/images/menkandi.png" mode=""></image>
					<image class="img" src="@/static/images/kefenqi.png" mode=""></image>
					<image class="img" src="@/static/images/menkandi.png" mode=""></image>
				</view>
			</view>
			<view class="footerBox">
				<!-- <text class="title1">粤ICP备19028584号-1</text> -->
				<text class="title2">理性贷款,避免逾期</text>
				<text class="title3">
					贷款有风险,借款需谨慎，请根据个人能力合理借款；实际贷款额度因个人情况和资质确定，资金来源于银行等正现放款机构。
				</text>
			</view>
		</view>
    
		<view v-if="isWeixin">
			<view class="wxtip" id="JweixinTip">
				<text class="wxtip-icon"></text>

				<text class="wxtip-txt">
					点击右上角
					<br />
					选择在浏览器中打开
				</text>
			</view>
		</view>
		
		<vStay v-if="showConfirm" @emitStay="eventStay"></vStay>
	</view>
</template>

<script>
	import myAnimateButton from '@/components/my-animate-button/my-animate-button.vue';
	import myModal from '@/components/my-modal/my-modal.vue';
    import vStay from "@/components/vStay";
	import $H from '@/common/lib/request.js';
	import $C from '../../common/lib/config.js';
	export default {
		components: {
			myAnimateButton,
			myModal,
			vStay
		},
		data() {
			return {
				is_send: false,
				time: 5,
				flag: true,
				selected: true,
				mobile: '',
				comments: [{
						id: 1,
						img: '../../static/avatar/4.jpg',
						mobile: '188****9434',
						desc: '现在这样的平台太多了，主个是我用过最放心的，赞~~'
					},
					{
						id: 2,
						img: '../../static/avatar/2.jpg',
						mobile: '183****1235',
						desc: '一点都不高！我借了18000，每月才还260元。'
					},
					{
						id: 3,
						img: '../../static/avatar/3.jpg',
						mobile: '147****3324',
						desc: '平台太多了，主个是我用过最放心的，赞~~'
					},
					{
						id: 4,
						img: '../../static/avatar/4.jpg',
						mobile: '188****2123',
						desc: '平台太多了，主个是我用过最放心的，赞~~'
					},
					{
						id: 4,
						img: '../../static/avatar/6.jpg',
						mobile: '188****2123',
						desc: '填了表就批了，你说呢？'
					},
					{
						id: 4,
						img: '../../static/avatar/2.jpg',
						mobile: '188****2123',
						desc: '在这贷款，最不怕的就是利息高'
					},
					{
						id: 4,
						img: '../../static/avatar/1.jpg',
						mobile: '188****2972',
						desc: '终于下款了，20000大元，终于下款了，20000大元'
					},{
					id: 4,
					img: '../../static/avatar/5.jpg',
					mobile: '178****6522',
					desc: '额度5000~申请5000~~一次性发放秒到，报着试试看态度搞的，今晚喝酒!'
				}],
				
				isWeixin: false,
				channelNo: '',
				showConfirm: false,
			};
		},

		mounted(val) {
			this.move();
			this.weixinTip();
			this.browserCount();
		},
		computed: {
			sendColor() {
				if (this.is_send) {
					return `color:#e5e5e5`;
				} else {
					return `color:#fff`;
				}
			}
		},
		created(){
			this.fobidden_back();
		},
		methods: {
			browserCount() {
				let channelNo = this.$route.query.channelNo || '';
				let clientType = 1; //默认安卓
				let u = navigator.userAgent;
				let isAndroid = u.indexOf('Android') > -1 || u.indexOf('Adr') > -1; //android终端
				let isiOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/); //ios终端
				if (isiOS) {
					clientType = 2;
				}
				console.log('clientType', clientType, 'channelNo', channelNo);
				$H.get(`/loan/ad/browserCount?clientType=${clientType}&channelNo=${channelNo}`, {}, {
					token: false
				});
			},

			weixinTip(ele) {
				var ua = navigator.userAgent;
				var isWeixin = !!/MicroMessenger/i.test(ua);
				this.isWeixin = isWeixin;
				if (isWeixin) {
					ele.onclick = function(e) {
						window.event ? (window.event.returnValue = false) : e.preventDefault();
						document.getElementById('JweixinTip').style.display = 'block';
					};
					document.getElementById('JweixinTip').onclick = function() {
						this.style.display = 'none';
					};
				}
			},
			jump() {
				uni.navigateTo({
					// url: '../detail/detail'
				});
			},
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
			policy(e) {
				console.log(e);
				if (e == 'reg') {
					uni.navigateTo({
						url: '../../components/register/register'
					});
				} else {
					uni.navigateTo({
						url: '../../components/privacy/privacy'
					});
				}
			},
			/**
			 * 滚屏
			 */
			move() {
				setInterval(() => {
					// let val=this.comments.shift();
					// console.log('val',val);
					this.comments.push(this.comments.shift());
				}, 1000);
			},
			/**
			 * @param {Object} 发送验证码
			 */
			sendSms(mobile) {
				$H.post(
					'/loan/api/user/getAuthCode', {
						authCodeType: 1,
						majia: 'jiguangfenqi',
						mobile
					}, {
						token: false
					}
				).then(res => {
					console.log('返回信息', res);
					const {
						status,
						msg
					} = res.data;
					if (status == '0') {
						return uni.showToast({
							title: msg,
							icon: 'none'
						});
					}
				});
			},
			// 接收更新手机号
			updateMobile(val) {
				this.mobile = val;
			},
			showModal() {
				if (!this.mobile) {
					return uni.showToast({
						title: '手机号不能为空',
						icon: 'none'
					});
				}
				if (!/^1[3456789]\d{9}$/.test(this.mobile)) {
					return uni.showToast({
						title: '请输入正确的手机号',
						icon: 'none'
					});
				}
				let clientType = 1; //默认安卓
				let u = navigator.userAgent;
				let isAndroid = u.indexOf('Android') > -1 || u.indexOf('Adr') > -1; //android终端
				let isiOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/); //ios终端
				if (isiOS) {
					clientType = 2;
				}
				$H.post(
					'/loan/api/common/checkUser', {
						mobile: this.mobile,
						clientType
					}, {
						token: false
					}
				).then(res => {
					const {
						status,
						data,
						msg
					} = res.data;

					if (data == 0) {
						//没有注册
						this.$refs.mymodal.popupDialogOpen();
						//直接下载
					} else if (data == 1) {
						uni.showToast({
							icon: 'none',
							title: msg
						});
						//开始注册
						let isiOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/); //ios终端
						const a = document.createElement('a'); // 创建a标签
						if (isiOS) {
							uni.navigateTo({
								url: '../downLoad/downLoad'
							});
						} else {
							a.setAttribute('download', 'apk'); // download属性
							a.setAttribute('href', 'https://test-1255867289.cos.ap-shanghai.myqcloud.com/apk/jiguang.apk'); // href链接
							a.click(); // 自执行点击事件
						}
					} else {
						uni.showToast({
							icon: 'none',
							title: msg
						});
					}
				});
			},
			  eventStay(obj) {
			      if (obj.type == "close") {
			        this.showConfirm = false;
			      } else if (obj.type == "come") {
			        this.showConfirm = false;
			      } else if (obj.type == "back") {
			        this.showConfirm = false;
			        history.go(-1);
			        window.removeEventListener("popstate", this.back_common);
			      }
			    },
			   //禁用浏览器返回
			    fobidden_back() {
			      //防止页面后退
			      history.pushState(null, null, document.URL);
			      window.addEventListener("popstate", this.back_common);
			    },
			    back_common() {
			      this.showConfirm = true;
			      history.pushState(null, null, document.URL);
			    }
		}
	};
</script>

<style lang="less" scoped>
	@import './index.less';

	.wxtip {
		background: rgba(0, 0, 0, 0.8);
		text-align: center;
		position: fixed;
		left: 0;
		top: 0;
		width: 100%;
		height: 100%;
		z-index: 998;
	}

	.wxtip-icon {
		width: 52px;
		height: 67px;
		background: url(../../static/images/live_weixin.png) no-repeat;
		display: block;
		position: absolute;
		right: 40upx;
		top: 20upx;
	}

	.wxtip-txt {
		display: block;
		margin-top: 107px;
		color: #fff;
		font-size: 16px;
		line-height: 1.5;
	}
</style>
