<template>
    <div class="container">
        <title-bar :title_name="$route.query.lotname" />
        <div class="message_box ellipsis_box" v-for="(l,index) in list" :key="index">
            <div> <span style="font-size:.4rem;color:#232323;margin-right:.2rem;">{{l.kjissue}}</span>  <span style="font-size:.32rem;color:#aaa;"> {{l.kjtime}}</span></div>
            <div v-if="l.kjnum && l.kjnum.length>0" style="margin-top:.2rem">
                <span v-for="(c,k) in l.kjnum.split(',')" :key="k" class="normal_style" :class="'color_'+c">{{c}}</span>
            </div>
        </div>

        

    </div>
</template>

<script>
import {getkjhis } from '@/api/home'
import { Dialog } from 'vant'
export default {
    data (){
        return {
            list:[],
        }
    },
    methods:{
        async getkjhis () {
          const { data }    = await getkjhis({
                lottype: this.$route.query.lottype
            });
          this.list = data.list;
        },
    },
    created(){
        this.getkjhis()
    },
} 
</script>

<style scoped lang="stylus">
.message_box
    padding .2rem
    border-bottom 1px solid #eee
.normal_style
    color #ffffff
    border-radius 50%
    width .65rem
    height .65rem
    display inline-block
    line-height .65rem
    text-align center
    margin-right .1rem
    font-size .39rem

.color_0
    background #EC808D
.color_1
    background #4B7902
.color_2
    background #0000FF
.color_3
    background #D9001B
.color_4
    background #6300BF
.color_5
    background #FF00C7
.color_6
    background #015478
.color_7
    background #F59A23
.color_8
    background #420080
.color_9
    background #6D000E
.color_10
    background #000000
.color_11
    background #02A7F0
</style>
