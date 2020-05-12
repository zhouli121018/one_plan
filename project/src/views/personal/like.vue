<template>
    <div class="container">
        <title-bar title_name="我的收藏" />
        <div class="message_box ellipsis_box" v-for="(l,index) in list" :key="index">
            <van-cell :title="l.planname"  :label="l.createtime" @click="go_home(l)">
                <div slot="right-icon">
                    <div>
                        <van-button size="mini" style="border-radius:.1rem;background:#108FE9;color:#fff;border-color:#108FE9;" @click.stop="del_like(l.user_plan_id)">删除</van-button>
                    </div>
                    <div>
                        <van-button size="mini" style="border-radius:.1rem;background:#108FE9;color:#fff;border-color:#108FE9;" @click.stop="show_newname(l)">改名</van-button>
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
            list:[],
            isFirstEnter:false,
            newname:'',
            show_tt:false,
            cur_user_plan_id:''
        }
    },
    methods:{
        go_home(obj){
            localStorage.setItem('lottype_one',obj.lottype);
            localStorage.setItem('playtype_one',obj.playtype);
            localStorage.setItem('pos_type_one',obj.pos_type);
            localStorage.setItem('mashu_one',obj.mashu);
            localStorage.setItem('qishu_one',obj.qishu);
            localStorage.setItem('user_plan_id_one',obj.user_plan_id);
            this.$router.push('/home/index')
        },
        async mylike () {
          const { data }    = await mylike();
          this.list = data.list;
        },
        show_newname(item){
            this.cur_user_plan_id = item.user_plan_id;
            this.newname = item.planname
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
                this.dislike();
            }).catch(() => {
            
            // on cancel
            });
        },
        async dislike(){
            const { data } = await dislike({
                user_plan_id: this.cur_user_plan_id
            })
            if(data.errorcode==0){
                this.mylike();
            }
        },
        async planname_modify(){
            const { data } = await planname_modify({
                user_plan_id: this.cur_user_plan_id,
                newname: this.newname
            })
            if(data.errorcode==0){
                this.mylike();
            }
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
