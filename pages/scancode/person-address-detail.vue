<template>
	<view class="container">
		<scroll-view class="examine-box" scroll-y style="height:100vh;">
			<view class="content">
				<block v-for="(items,index) in dataArr" :key="items.id">
					<view class="row-list">
						<view class="title">
							<view class="dot-box">
								<view class="dot" :class="(items.checkStatus === '0' || items.checkStatus === '1') ? 'bg-red' : (items.checkStatus === '2' ? 'bg-green' : '')"></view>
							</view>
							<text class="title-label">出入地址{{index + 1}}: </text>
							<text class="title-address">{{items.address}}</text>
						</view>
						<view class="remark" v-if="items.checkStatus === '0'">说明: 待审核，请耐心等待审核！</view>
						<view class="remark" v-else-if="items.checkStatus === '1'">说明: 审核不通过（需要重新审核）！</view>
						<view class="remark" v-else-if="items.checkStatus === '2'">说明: 有效期至 {{items.endTime}} ！</view>
					</view>
				</block>
			</view>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				dataArr: []
			};
		},
		onLoad() {
			this.dataArr = this.$store.state.registerData
		}
	}
</script>

<style lang="scss">
	page {
		background: #fff;
		height: 100%;
		
		font-size: $fz16;

		.person-info {
			border-bottom: 1px solid $b-t;
		}
		
		// 列表
		.row-list {
			display: flex;
			padding: 30rpx 20rpx;
			align-items: baseline;
			flex-direction: column;
			box-sizing: border-box;
			border-bottom: 1px solid #E4E4E4;
			.title {
				display: flex;
				.title-label {
					min-width: calc(4em + 15px);
					color: $color6;
				}
				.title-address {
					flex: 1
				}
			}
			.dot-box {
				box-sizing: border-box;
				padding: 5rpx 30rpx 5rpx 5rpx;
				.dot {
					width: 30rpx;
					height: 30rpx;
					border-radius: 50%;
				}
				.bg-red {
					background-color: red;
				}
				.bg-green {
					background-color: green;
				}
			}
			.remark {
				font-size: $fz14;
				padding: 30rpx 0 10rpx 70rpx;
				color: #7f7f7f;
			}
		}
		.row-list:last-of-type {
			border-bottom: unset;
		}
	}
</style>
