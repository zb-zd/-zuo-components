<template>
    <view class="notice-list" :position="position">
        <view 
            class="notice-list-item" 
            :class="item.visible ? 'notice-show' : 'notice-hide'"
            :style="{width: item.width, marginTop: item.marginTop}"
            @click.stop="onClickNoticeItem(item)"
            v-for="(item, index) in msgList" 
            :key="index"
        >
            <NoticeItem :message="item.message" />
        </view>    
    </view>
</template>

<script>
    import NoticeItem from './notice-item.vue';

    export default {
        props: {
            position: {
                type: String,
                default: 'top-right' // top-left, top-right, bottom-left, bottom-right
            }
        },
        components: {
            NoticeItem
        },
        data() {
            return {
                msgList: []
            }
        },
        methods: {
            add({ message = '', autoHide= true, type = 'top-right' }) {
                const msgList = this.msgList;
                this.msgList.push({
                    order: msgList.length + 1,
                    message,
                    autoHide,
                    visible: true,
                    width: `${70 - (msgList.length * 2)}vw`,
                    marginTop: `${msgList.length * 2}%`
                })
                if(autoHide) this.countDown(msgList.length - 1);
            },
            countDown(curIndex) {
                this.msgList[curIndex].countTime = setTimeout(() => {
                    this.msgList[curIndex].visible = false
                    clearTimeout(this.msgList[curIndex].countTime)
                }, 2800)
            },
            onClickNoticeItem(data) {
                data.visible = false;
                this.$emit('clickChild', data)
            }
        }
    }
</script>

<style lang="scss" scoped>
    .notice-list {
        position: fixed;
        width: 70vw;
        margin-top: 24rpx;
        &-item {
            visibility: hidden;
            position: absolute;
            margin-top: 24rpx;
        }
        .notice-show {
            visibility: visible;
            margin-right: -100%;
            animation-duration: 0.2s !important;
            animation-timing-function: ease-in-out !important;
            animation-fill-mode: forwards !important;
        }
        .notice-hide {
            visibility: visible;
            right: 32rpx;
            animation-duration: 0.2s !important;
            animation-timing-function: ease-in-out !important;
            animation-fill-mode: forwards !important;
        }
        &[position=top-right], &[position=top-left] {
            top: calc(var(--status-bar-height) + 44px);
            right: 0;
            .notice-list-item {
                right: 32rpx;
            }
            .notice-show {
                margin-right: -100%;
                animation: innoticeTopRight;
            }
            .notice-hide {
                right: 32rpx;
                animation: outnoticeTopRight;
            }
        }
        &[position=top-left], &[position=bottom-left] {
            bottom: 128rpx;
            left: 0;
            .notice-list-item {
                left: 32rpx;
            }
            .notice-show {
                margin-left: -100%;
                animation: innoticeTopLeft;
            }
            .notice-hide {
                left: 32rpx;
                animation: outnoticeTopLeft;
            }
        }
    }

    // top-right
    @keyframes innoticeTopRight {
        from {
            margin-right: -100%; 
        }
        to {
            margin-right: 0;
        }
    }

    @keyframes outnoticeTopRight {
        from {
            margin-right: 0;
        }
        to {
            margin-right: calc(-100% - 32rpx);
        }
    }

    // top-left
    @keyframes innoticeTopLeft {
        from {
            margin-left: -100%; 
        }
        to {
            margin-left: 0;
        }
    }

    @keyframes outnoticeTopLeft {
        from {
            margin-left: 0;
        }
        to {
            margin-left: calc(-100% - 32rpx);
        }
    }
</style>