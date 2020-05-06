<template>
    <div class="container">
        <title-bar title_name="我的收藏" />
        <div class="message_box ellipsis_box" v-for="(l,index) in list" :key="index">
            <van-cell :title="l.planname"  :label="l.createtime" >
                <div slot="right-icon">
                    <div>
                        <van-button type="danger" size="mini" style="border-radius:.1rem;" @click="del_like(l.id)">删除</van-button>
                    </div>
                    <div>
                        <van-button type="warning" size="mini" style="border-radius:.1rem;" @click="show_newname(l.id)">改名</van-button>
                    </div>
                </div>
            </van-cell>
        </div>

        <van-dialog 
            v-model="show_tt"
            title="改名"
            show-cancel-button
            class="dialog_content_input"
            :before-close="beforeClose"
            >
            <van-field
                v-model.trim="newname"
                clearable
                label="名称："
            />
        </van-dialog>

    </div>
</template>

<script>
import {mylike, dislike, planname_modify } from '@/api'
import { Dialog } from 'vant'
export default {
    data (){
        return {
            list:[{planname:'测啊测啊当测啊测啊当测啊测啊当测啊测啊当时是当时是时是当时是测啊测啊当时是当时是时是当时是测啊测啊当测啊测啊当时是当时是时是当时是测啊测啊当时是当时是时是当时是',id:0,createtime:'2020-05-06'},{planname:'测啊当时是',id:0,createtime:'2020-05-06 11:00:00'},{planname:'测啊当时是',id:0,createtime:'2020-05-06'}],
            isFirstEnter:false,
            newname:'',
            show_tt:false,
            cur_user_plan_id:''
        }
    },
    methods:{
        async mylike () {
          const { data }    = await mylike({
                sid: localStorage.getItem('sid'),
                uid: localStorage.getItem('uid')
            });
        //   this.list = data.list;
        },
        show_newname(id){
            this.cur_user_plan_id = id;
            this.show_tt = true;
        },
        beforeClose(action,done){
            if(action == 'confirm'){
                if(!this.newname){
                    this.$toast('请输入名称！')
                    done(false)
                    return;
                }
                this.planname_modify();
                this.newname = ''
            }
            done();
        },
        goDetail(data){
            this.$router.push({
                path: '/home/announcement/detail', 
                query: {
                    // title: data.title, 
                    noticeid: data.id
                }
            })
        },
        del_like(id){
            Dialog.confirm({
                title: '温馨提示',
                cancelButtonText:'关闭',
                confirmButtonText:'删除',
                message: '确认删除吗？'
            }).then(() => {
                // on confirm
                this.cur_user_plan_id = id
                this.like();
            }).catch(() => {
            
            // on cancel
            });
        },
        async dislike(){
            const { data } = await dislike({
                sid: localStorage.getItem('sid'),
                uid: localStorage.getItem('uid'),
                user_plan_id: this.cur_user_plan_id
            })
        },
        async planname_modify(){
            const { data } = await planname_modify({
                sid: localStorage.getItem('sid'),
                uid: localStorage.getItem('uid'),
                user_plan_id: this.cur_user_plan_id,
                newname: this.newname
            })
        }
    },
    created(){
        this.isFirstEnter=true;
    },
    activated(){
        if(this.$route.meta.isBack){
            this.$store.dispatch('set_isback',true)
        }
        this.$route.meta.isBack=false;
        if(!this.$store.getters.isback || this.isFirstEnter){
            this.mylike()
        }
        this.isFirstEnter=false;
        this.$store.dispatch('set_isback',false)
    },
} 
</script>

<style scoped lang="stylus">
.message_box
    border-bottom 1px solid #eee
    
</style>
