<template>
    <div class="container" v-if="info != null">
        <title-bar title_name="微信二维码支付" />
        <div class="pay_title">
            <img src="~@/assets/wechat@2x.png" alt="">
            <div>
                <p>微信二维码支付</p>
                <p style="padding-top:0.1rem">微信安全支付</p>
            </div>
        </div>
        <h4 style="text-align:center;font-weight:bold;font-size:0.5rem;margin-top:0.2rem;">付款备注: {{info.beizu}}</h4>
        <div class="red pay_prompt">
            {{info.noticedesc}}
        </div>
        <div class="pay_code">
            <div class="qrcode">
                <img :src="$https+info.barcodeurl" alt="">
            </div>
            <p>充值失败联系微信:{{$store.getters.kfwecha}}</p>
        </div>
        <div class="xian"></div>
        <div class="simulate_rules">
            <h4 class="rules_h">步骤:</h4>
            <p>1.截屏保存图片</p>
            <p>2.打开微信扫一扫选择相册</p>
            <p>3.选择刚刚保存的二维码</p>
            <p>4.付款的时候添加备注</p>
        </div>
    </div>
</template>

<script>
import { getwechatcode } from '@/api'
export default {
    data() {
        return {
            money: 0,
            type: '',
            info: null,
            isFirstEnter:false
        } 
    },
    methods: {
        async getwechatcode() {
            const { data } = await getwechatcode({
                money: this.money,
                type: this.type//0购买金币，1开通会员
            })
            this.info = data
        }
    },
    created() {
        this.isFirstEnter=true;
        this.type = this.$route.query.type
        this.money = this.$route.query.money
        this.getwechatcode()
    },
}
</script>

<style lang="stylus" scoped>
.simulate_rules
    width 100%
    padding .2rem .32rem
    box-sizing border-box
    text-align left 
    p
        line-height .45rem
        font-size .3rem
        color #666
.pay_code 
    width 100%
    text-align center
    margin-top .3rem
    padding-bottom .4rem
    .qrcode
        width 4rem
        height 4rem
        margin .3rem auto
    img 
        width 4rem
        height 4rem
    p
        font-size .32rem
        color #666
.pay_prompt
    padding .32rem
    box-sizing border-box
    font-size .38rem
    line-height .5rem
h4
    width 100%
    padding .2rem 0
    font-size .4rem
.pay_title
    width 100%
    padding .2rem .3rem
    box-sizing border-box
    border-bottom 1px solid #cccccc
    display flex
    align-items center
    img 
        width .8rem 
        height .8rem
        margin-right .4rem
        display inline-block
        border 1px solid #D6CBC8
        border-radius .15rem
    p
        line-height .5rem
        &:last-child
            font-size .36rem
            color #666
</style>
