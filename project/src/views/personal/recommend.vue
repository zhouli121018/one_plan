<template>
    <div class="container" v-if="info">
        <div style="position:relative;">
            <img src="~@/assets/tuijian.png" alt="" style="width:100%;">
            <div style="position:absolute;top:.3rem;left:.3rem;padding:.15rem;" @click="goBack">
                <img src="~@/assets/returnpre.png" alt="" style="width:.26rem;">
            </div>
            <div style="position:absolute;top:1.5rem;left:.65rem;color:#DFE9FD;font-size:0.4rem;line-height:1.4;" v-html="info.leftstr"></div>
        </div>
        <div style="padding:.1rem .3rem;position:relative;">
            <img src="~@/assets/tuijian_bg.png" alt="" style="width:100%;">
            <div style="position:absolute; top:1.3rem;left:1.2rem;right:1.8rem;text-align:center;" class="flex">
                <div class="flex_grow_1">
                    <div  style="color:#DFE9FD;font-size:.39rem;">X码X期任你选</div>
                    <div style="font-size:0.35rem;color:#DFE9FD;padding:.2rem 0;">「一个计划」打遍天下无敌手</div>
                    <van-button style="width:3.2rem;background:#DFE9FD;color:#000000;font-size:0.35rem;border-radius:.02rem;height:.6rem;line-height:.4rem;">扫码免费领</van-button>
                </div>
                <img :src="$https+info.barcode" alt="" style="width:1.92rem;height:1.92rem;">
            </div>
        </div>
        <div style="padding:.4rem .8rem">
            <div  class="bg_alpha">
            <!-- <img src="~@/assets/tuijian_alpha.png" alt=""> -->
                <div>{{info.title_list}}</div>
                <div v-for="(l,index) in info.list" :key="index">{{l}}</div>
            </div>
        </div>
    </div>
</template>

<script>
import { gettuijiancode, getsharedata } from '@/api/home'
export default {
    data() {
        return {
            info: null
        }
    },
    methods: {
        // 返回
        goBack(){
            this.$store.dispatch('set_isback',true)
            setTimeout(() => {
                this.$store.dispatch('set_isback',false)
            }, 500);
            this.$router.go(-1)
        },
        async gettuijiancode() {
            const { data } = await gettuijiancode()
            if(data.errorcode == 0) {
                this.info = data
            }
        },
        async getsharedata() {
            const { data } = await getsharedata()
            if(data.errorcode == 0) {
                this.info = data
            }
        }
    },
    created() {
        this.getsharedata()
    }
}
</script>

<style lang="stylus" scoped>
.container
    padding-top 0!important
    background #5385F2
.bg_alpha
    background url(../../assets/tj_alpha.png)
    border-radius .2rem
    padding .5rem .2rem
    line-height 1.6
    text-align center
    color #DFE9FD
    font-size .32rem
</style>

