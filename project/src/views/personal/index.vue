<template>
    <div class="container">
        <title-bar title_name="会员中心" />
        <div class="" style="margin: 0.4rem 0.3rem;border-radius: 0.2rem;background: #fff;"  v-if="info != null">
            <div class="my_title">
                <div class="flex_grow_1">
                    <div class="flex">
                        <img class="my_title_photo" src="~@/assets/personal/defaulticon.png" alt="">
                        <div class="my_title_center">
                            <div>
                                <span style="font-weight:bold;font-size:0.4rem;color:#232323;">账号：{{info.account}}</span>
                            </div>
                            <div class="goldcoins_fans">
                                <!-- <p><span>金币: </span><span class="red">{{info.coin}}</span></p>
                                <p><span>粉丝: </span><span class="red">{{info.fans}}</span></p> -->
                                <!-- $isvip='1';//是否VIP 1是vip，0不是vip -->
                                <!-- <img v-if="info.isvip == 1" class="my_title_center_img" src="~@/assets/vip.png" alt=""> -->
                                <!-- <img v-else class="my_title_center_img" src="~@/assets/vip-gary.png" alt=""> -->
                                <span class="red" style="font-size:.37rem;">会员天数：{{info.viptime}}</span>
                                <!-- <span v-else class="red">(非会员)</span> -->
                            </div>
                        </div>
                    </div>
                    
                </div>

                
                <!-- <van-button v-if="info.isvip == 0" class="orange_btn" round @click="jumpTo('/home/openingMember')" style="white-space:nowrap;">开通会员</van-button> -->
                <van-button class="orange_btn" round @click="jumpTo('/personal/freeUse')" style="white-space:nowrap;">免费获会员天数</van-button>
                
            </div>
            <div>
                <div style="color:#989898;font-size:.29rem;padding:0 .35rem .3rem">{{info.vipdes}}</div>
            </div>
        </div>
        
        

        <div class="my_title_box" v-if="false">
            <div class="my_title flex" style="border:none;width:47.5%" v-if="info">
                <img class="my_title_photo title_photo" src="~@/assets/icon.png" alt="">
                <div class="my_title_center my_centers flex_grow_1" style="padding-left:.1rem;">
                    <p style="padding-bottom:.1rem">
                        <b style="font-size:0.5rem;font-weight:bold;">{{info.income_cur}}元</b>
                    </p>
                    <!-- <p class="goldcoins_fans">
                        佣金金额
                    </p> -->
                    <van-button type="danger" size="small" @click="show = true" >返佣提款</van-button>
                </div>
                
            </div>
            <div class="my_title flex" style="border:none;width:51%" v-if="info">
                <img class="my_title_photo title_photo" src="~@/assets/ticketnum.png" alt="">
                <div class="my_title_center my_centers flex_grow_1" style="padding-left:.1rem;">
                    <p style="padding-bottom:.1rem">
                        <b style="font-size:0.5rem;font-weight:bold;">{{info.invitenum}}人</b>
                    </p>
                    <!-- <p class="goldcoins_fans">
                        邀请人数
                    </p> -->
                    <van-button type="danger" size="small" @click="jumpTo('/personal/inviteDetail')" >邀请明细</van-button>
                </div>
                
            </div>
        </div>
        <!-- <div class="xian"></div>         -->
        <div style="margin:0.4rem 0.3rem;border-radius:.2rem;overflow: hidden;">
            <van-cell title="邀请明细" is-link icon="jifen" @click="jumpTo('/personal/inviteDetail')"/>
            <van-cell title="我的推荐页" is-link icon="tj" @click="jumpTo('/personal/recommend')"/>
            <van-cell title="免费使用" is-link icon="freeuse" @click="jumpTo('/personal/freeUse')"/>
            <van-cell title="群发计划" is-link icon="qunfa" @click="jumpTo('/home/announcement/detail?noticeid=1')"/>
            <van-cell title="我的收藏" is-link icon="like"  @click="jumpTo('/personal/like')"/>
            <van-cell title="关于" is-link icon="about"  @click="jumpTo('/personal/about')"/>
        </div>


        <div class="text_center">
            <van-button @click="logout" size="large"  style="border:0;font-size:.4rem;color:#232323;">退出帐号</van-button>
        </div>

        <van-dialog 
            v-model="show"
            title="提款提示"
            show-cancel-button
            class="dialog_content_input"
            :before-close="beforeClose_tk"
            >
            <van-field
                v-model.trim="alipay"
                clearable
                label="支付宝："
            />
        </van-dialog>
        <van-dialog 
            v-model="isShow"
            title="兑换会员天数"
            show-cancel-button
            class="dialog_content_input"
            :before-close="beforeClose"
            >
            <van-field
                v-model.trim="vipticket"
                clearable
                label="会员券(张)："
                type="number"
            /> 
        </van-dialog>
    </div>
</template>

<script>
import { getaccount, submitduihuan } from '@/api'
import { submittikuan } from '@/api/home'
import { Dialog } from 'vant'
export default {
    data() {
        return {
            info: null,
            isFirstEnter:false,
            show:false,
            alipay:'',
            isShow: false,
            ticketnum: '',
            vipticket: ''
        }
    },
    methods:{
        logout(){
            Dialog.confirm({
                className:'small_padding',
                message: '退出当前帐号？'
            }).then(() => {
                // on confirm
                // localStorage.clear();
                localStorage.removeItem('uid_one')
                localStorage.removeItem('sid_one')
                this.$router.push('/home/index')
            }).catch(() => {
                // on cancel
            });
            
        },
        beforeClose_tk(action,done){
            if(action == 'confirm'){
                if(!this.alipay){
                    this.$toast('请输入支付宝账号！')
                    done(false)
                    return;
                }
                this.submittikuan();
                this.alipay = ''
            }
            done();
        },
        async submittikuan() {
            const { data } = await submittikuan({
                alipay:this.alipay
            })
            this.info.income_cur = data.yongjin
        },
        jumpTo(path){
            this.$router.push(path);
        },
        async getaccount() {
            const { data } = await getaccount({})
            this.info = data
            this.ticketnum = this.info.ticketnum
        },
        goAbout(){
            this.$router.push('/personal/about');
        },
        beforeClose(action,done){
            if(action == 'confirm'){
                if(!this.vipticket){
                    this.$toast('请输入会员券!')
                    done(false)
                    return;
                }
                this.submitexchangeDay();
                this.vipticket = ''
            }
            done();
        },
        //兑换会员天数
        async submitexchangeDay() {
            const { data } = await submitduihuan({
                vipticket: this.vipticket
            }) 
            this.ticketnum = data.ticketnum//兑换后剩余优惠券张数
            // this.$toast(data.message)
            if(data.errorcode == 0){
                this.getaccount();
            }
        },
    },
    created() {
        this.isFirstEnter=true;
    },
    beforeRouteEnter(to, from, next) {
      if (from.name == 'openingMember' || from.name == 'recommend' || from.name == 'earnMoney' || from.name == 'openRemind' || from.name == 'freeUse' || from.name == 'about') { // 这个name是下一级页面的路由name
        to.meta.isBack = true; 
      }
      next()
    },
    activated(){
        if(this.$route.meta.isBack){
            this.$store.dispatch('set_isback',true)
        }
        this.$route.meta.isBack=false;
        this.getaccount();
        this.isFirstEnter=false;
        this.$store.dispatch('set_isback',false)
    },
}
</script>

<style scoped lang="stylus">
.xian
    background #f4f4f4
    height .4rem
.text_center
    padding 0 .3rem
    text-align center
.container
    background #F4F4F4
.my_centers
    width 60%!important
    p 
        line-height .6rem
        &:last-child
            color #666
            font-size .35rem
/deep/ .van-cell
    line-height .88rem
    
   
/deep/ .van-icon-jifen::before
    content ''
    width .88rem
    height .88rem
    background url('~@/assets/personal/jifendetail.png') no-repeat
    background-size contain

/deep/ .van-icon-tj::before
    content ''
    width .88rem
    height .88rem
    background url('~@/assets/personal/tuijian.png') no-repeat
    background-size contain

/deep/ .van-icon-freeuse::before
    content ''
    width .88rem
    height .88rem
    background url('~@/assets/personal/freeuse.png') no-repeat
    background-size contain

/deep/ .van-icon-qunfa::before
    content ''
    width .88rem
    height .88rem
    background url('~@/assets/personal/qunfa.png') no-repeat
    background-size contain

/deep/ .van-icon-like::before
    content ''
    width .88rem
    height .88rem
    background url('~@/assets/personal/like.png') no-repeat
    background-size contain

/deep/ .van-icon-about::before
    content ''
    width .88rem
    height .88rem
    background url('~@/assets/personal/about.png') no-repeat
    background-size contain

.my_title
    // width: 100%
    padding .4rem .35rem .2rem
    box-sizing border-box 
    display flex
    align-items center
    // border-bottom 1px solid #f0f0f0
    // background #fff
    // margin-top:-.1rem
    // margin .4rem .3rem
    border-radius .2rem
    .my_title_center
        span 
            line-height .5rem
            padding-right .1rem
    .my_title_photo
        width 1.5rem
        height 1.5rem
        border-radius 50%
        margin-right .1rem
    .goldcoins_fans
        display flex
        padding-top .1rem
        align-items center
        span 
            font-size .26rem
        p
            padding-right .3rem
    .my_title_center_img
        width .8rem
        height .7rem
        margin 0 .2rem 0 0
.orange_btn
    border-radius .6rem
    background #FFC131
    color #FF3858
    height 1rem
    line-height 1rem
    padding 0 .3rem
    font-size 0.36rem
.title_photo
    width .77rem!important
    height .77rem!important
.my_title_box
    width 100%
    background #eee
    padding .4rem 0
    box-sizing border-box
    display flex
    justify-content space-between
    align-items center
    .my_title
        width 49%
        padding .3rem .1rem
        .my_centers
            width 45%!important
    /deep/ .van-button--small {
        padding: 0 6px;
        height: 25px;
        min-width: 45px;
        font-size: 13px;
        line-height: 20px;
    }
</style>
