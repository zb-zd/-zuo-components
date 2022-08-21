<template>
	<view class="content">
		<Notice ref="notice" @clickChild="onClickChild" />
	</view>
</template>

<script>
	import Notice from '@/components/z-notice/notice.vue'
	export default {
		components: {
			Notice
		},
		onLoad() {
			this.$nextTick(() => {
				this.$refs.notice.add({ message: '我是消息标题我是消息标题我是消息标题', autoHide: false })
				setTimeout(() => {
					this.$refs.notice.add({ message: '我是消息标题2', autoHide: false })
				}, 1000)
			})
		},
		methods: {
			onClickChild(data) {
				console.log(data)
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
		margin: 200rpx auto 50rpx auto;
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
