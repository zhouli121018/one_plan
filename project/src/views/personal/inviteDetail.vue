<template>
    <div class="container">
        <title-bar title_name="邀请明细" />
        <div>
            <div class="invite_title">
                <div style="color:#74B0E0;font-size:0.37rem;">
                    总天数
                </div>
                <div style="color:#FFFFFF;font-size:0.8rem;padding-top:.35rem;">
                    {{day_sum}} <span style="color:#74B0E0;font-size:0.37rem;">天</span>
                </div>
            </div>
            <table style="width:100%;" v-if="list.length>0">
                <tr>
                    <td class="head_td" style="padding-left:.2rem;">时间</td>
                    <td class="head_td">来源</td>
                    <td class="head_td">天数</td>
                </tr>
                <!-- <tr >
                    <td style="padding-left:.2rem">2019/01/01 19:19:12</td>
                    <td>176****9682</td>
                    <td>已开通会员</td>
                </tr>
                <tr >
                    <td style="padding-left:.2rem">2019/01/01 19:19:12</td>
                    <td>176****9682</td>
                    <td>还差一个</td>
                </tr> -->
                <tr v-for="(l,index) in list" :key="index">
                    <td style="padding-left:.2rem">{{l.createtime}}</td>
                    <td>{{l.username}}</td>
                    <td>{{l.days}}</td>
                </tr>
            </table>
        </div>
    </div>
</template>

<script>
import {getinvitelist } from '@/api'
export default {
    data(){
        return {
            title_name: '邀请明细',
            title:'',
            list:[],
            content:'',
            day_sum:0,
        }
    },
    methods:{
        async getinvitelist() {
            let obj = {};
            const { data } = await getinvitelist(obj);
            this.list = data.list;
            this.day_sum = data.day_sum;
        },
    },
    created(){
        this.getinvitelist();
    }
}
</script>

<style  lang="stylus" scoped>
.invite_title
    background linear-gradient(90deg,#1B82D2,#1B82D2)
    padding .3rem 0 .8rem .45rem
.container
    padding-top: 46px !important;
table td.head_td{
    background:#F5F5F5;
    font-weight:bold;
    font-size:0.37rem;
    color:#333333;
}
table td{
    padding-top:.2rem;
    padding-bottom:.2rem;
    font-size:.35rem
    color:#989898;
    padding:.4rem;
    border-bottom:.03rem solid #f5f5f5;
}
/deep/ .van-hairline--bottom::after
        border-bottom-width: 0
    
</style>
