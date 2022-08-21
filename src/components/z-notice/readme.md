# 使用方式
```vue
<view>
    <Notice ref="notice" @clickChild="onClickChild" />
</view>
<script>
    import Notice from '@/components/z-notice/notice.vue'
    export default {
        components: {
            Notice
        },
		onLoad() {
			this.$nextTick(() => {
				this.$refs.notice.add({ message: '我是消息标题我是消息标题我是消息标题', autoHide: true })
				setTimeout(() => {
					this.$refs.notice.add({ message: '我是消息标题2', autoHide: true })
				}, 1000)
			})
		},
        methods: {
            onClickChild(data) {
                console.log('点击消息回调函数', data);
            }
        }
    }
</script>

```