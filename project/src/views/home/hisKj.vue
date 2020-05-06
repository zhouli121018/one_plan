<template>
    <div class="container">
        <title-bar title_name="历史开奖" />
        <div class="message_box ellipsis_box" v-for="(l,index) in list" :key="index">
            {{l.kjnum}}
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
                sid: localStorage.getItem('sid'),
                uid: localStorage.getItem('uid'),
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
    border-bottom 1px solid #eee
    
</style>
