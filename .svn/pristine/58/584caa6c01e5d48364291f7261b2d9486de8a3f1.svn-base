<template>
	<view class="content">
		<image src="../../static/code-bg.png" mode=""></image>
		<!-- 人员信息 -->
		<scroll-view class="top-box" scroll-y style="height:100%;">
			<!-- 二维码区域 -->
			<view class="code-box">
				<!-- <canvas canvas-id="qrcode" style="margin: 0 auto;width: 215px;height: 215px;" /> -->
				<image :src="qrcode" mode="" style="width: 215px;height: 215px;"></image>
				<!--  -->
				<view v-if="status==1" class="btn-blue btn-box">
					<view>通行证有效</view>
				</view>
				<view v-if="status==0" class="btn-red btn-box">
					<view>通行证无效</view>
				</view>
				<view v-if="status==1">有效期至{{data.endTime}}</view>
				<view v-if="status==0" class="btn-explain">无效说明:{{data.appMsg}}</view>
			</view>
			<!-- 个人信息 -->
			<view class="input-box">
				<view class="label">姓&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;名:</view>
				<view class="text">{{data.xm | tp}}</view>
			</view>
			<view class="input-box">
				<view class="label">电话号码:</view>
				<view class="text">{{data.lxdh | tp}}</view>
			</view>
			<view class="input-box">
				<view class="label">身份证号:</view>
				<view class="text">{{data.zjhm | rp}}</view>
			</view>
			<view class="input-box">
				<view class="label">居住地址:</view>
				<view class="text">{{data.address | tp}}</view>
			</view>
		</scroll-view>
		 <!-- 消息提示组件 -->
		 <message ref="Message" :duration="2000" background></message>
	</view>
</template>

<script>
	import uQRCode from '@/common/uqrcode.js'
	import req from '@/common/req.js'
	export default {
		data() {
			return {
				// 通行证状态
				status:1,
				// 数据信息
				data:"",
				//二维码
				qrcode:""
			};
		},
		onLoad() {
			// 初始二维码
			this.loadQrcode();
		},
		methods: {
			// 初始二维码
			async loadQrcode(){
				uni.showLoading({
					mask:true,
				    title: "正在加载中"
				});
				let result = await this.$request.post(this.$api.getQCcode,{"openId":this.$store.state.openId});
				// 通行证状态--- 1有效  0 无效 
				this.status =  result.appCode;
				// 
				if(result.appCode == 1 || result.appCode == 0){
					result.data.appMsg = result.appMsg;
					this.data = result.data;
					this.qrcode = "data:image/png;base64,"+result.QRcode;
					this.make(result.data.id);
				}else{
					let _this = this;
					this.$refs['Message'].error(result.appMsg);
					let timeout = setTimeout(function() {
						_this.$routes.navBack();
						clearTimeout(timeout);
					}, 3000);
				}
				uni.hideLoading();
				
			},
			// 生成二维码
			 make(text) {
			    uQRCode.make({
			        canvasId: 'qrcode',
			        componentInstance: this,
			        text: text,
			        size: 215,
			        margin: 10,
			        backgroundColor: '#ffffff',
			        foregroundColor: '#000000',
			        fileType: 'jpg',
			        correctLevel: uQRCode.defaults.correctLevel,
			        success: res => {
			          console.log(res)
			        }
				}
			)},
			// 个人信息
			navPerson(){
				// 保存数据到状态
				this.$store.dispatch('refreshPersonData',this.data);
				// 
				this.$routes.navTo('/pages/scancode/person-info');
			},
			// 通行记录
			navRecord(){
				this.$routes.navTo('/pages/scancode/current-record');
			}
		}
	}
</script>

<style lang="scss">
	page,.content{
		height: 100%;
		background:#fff;
		// 二维码
		image{
			height: 100%;
			width: 100%;
		}
		// 扫码人员信息
		.top-box{
			position: fixed;
			z-index: 10;
			top:0;
			left:0;
			right:0;
			padding-top:20px;
			text-align: center;
			view{
				margin-bottom: 15px;
			}
			// 二维码区域
			.code-box{
				width: 260px;
				margin: 20px auto 40px;
				padding: 10px;
				background: #fff;
			}
			// 按钮盒子
			.btn-box{
				text-align: center;
				margin-bottom:0;
				margin-top:10px;
				view{
					display: inline-block;
					width: 215px;
					height: 40px;
					line-height: 40px;
					border-radius: 70px;
					color: #fff;
				}
			}
			.btn-blue{
				view{
					background-color: rgb(135,245,10);
				}
			}
			.btn-red{
				view{
					background-color: rgb(251,54,6);
				}
			}
			// 说明
			.btn-explain{
				color: rgb(252,86,54);
			}
			// 个人信息区域
			.input-box{
				display: flex;
				background: #fff;
				margin:0 30px 10px;
				padding:0 10px;
				border-radius:5px;
				view{
					margin-bottom: 0;
					padding: 8px 0;
				}
				// 两边对齐处理
				.label{
					color: rgb(53,96,202);
					width: calc(4em + 5px);
					text-align: justify;
					margin-right:10px;
					// &::after {
					// 	display: inline-block;
					// 	width: 100%;
					// 	content: '';
					// 	height: 0;
					// }
				}
				.text{
					flex:1;
					text-align: left;
				}
			}
		}
		
		
	}
</style>
