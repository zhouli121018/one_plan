<template>
    <div class="container">
        <title-bar title_name="新用户注册" right_text="登录" right_url="/login/index"/>
        <div class="van_box">
            <van-field label="帐号"  clearable v-model="phone" placeholder="请输入帐号" />
        </div>
        <div class="van_box">
            <van-field label="密码" type="number" maxlength="11" clearable v-model="password" placeholder="需6位数字密码" />
        </div>
        <!-- <div class="van_box">
            <van-field label="验证码" maxlength="11" type="number" class="van_field" clearable v-model="vcode" placeholder="请输入验证码" />
            <CutDown @click="codeVerify" :disabled="disabled" :mobile="phone" ref="codeEl"></CutDown>
        </div> -->
        <div class="van_box">
            <van-field label="邀请码" maxlength="11" type="number" :disabled="has_pid" class="van_field_code" clearable v-model="pid" :placeholder="regpiddes" />
        </div>
        <van-button style="background:#108FE9;color:#fff;border-radius:.1rem;" @click="regist">注册</van-button>
        <div style="text-align:right;width:90%;margin:0 auto;padding-top:.3rem;">
            <span style="color:#108FE9;font-size:.4rem;" @click="go_login">登录</span>
        </div>
    </div>
</template>

<script>
import { validatePhone } from '@/utils/validate'
import CutDown from '@/components/CutDown'
import { getvcode, regist } from '@/api'
import { gethome } from '@/api/home'
import { Toast } from 'vant';
export default {
    components: {
        CutDown
    },
    data() {
        return {
            phone: '',
            pid: '', //邀请码
            password: '',
            vcode: '', //验证码
            device: 0  ,//手机类型,
            has_pid:false,
            regpiddes:'邀请码'
        }
    },
    methods: {
        go_login(){
            this.$router.push('/login/index');
        },
        async codeVerify() {
            const { data } = await getvcode({
                phone: this.phone
            })
            if(data.errorcode == 0){
                this.$refs.codeEl.isStart = true;
                this.$refs.codeEl.start();
            }
            this.$toast(data.message)
            
        },
        async regist() {
            if(!this.phone){
                Toast('请输入帐号！');
                return;
            }
            if(!this.password){
                Toast('请输入密码！');
                return;
            }
            let obj = {
                phone: this.phone,
                pass: this.password,
                device: this.device,
                pid: this.pid
            };
            if(localStorage.getItem('cid_one')){ //渠道号
                obj.cid = localStorage.getItem('cid_one')
            }
            const { data } = await regist(obj)
            if(data.errorcode == 0) {
                window.localStorage['uid_one'] = data.uid
                window.localStorage['sid_one'] = data.sid
                this.$router.replace('/home/index')
            }
        }
    },
    computed: {
        disabled() {
            return !validatePhone(this.phone)
        },
        submitValidate() {
            if(!this.phone || !this.code) {
                return {
                    ok: false,
                    msg: '请填写完整信息'
                }
            }
            if(!validatePhone(this.phone)) {
                return {
                    ok: false,
                    msg: '请输入正确手机号'
                }
            }
            return {
                ok: true,msg: 'ok'
            }
        }
    },
    created(){
        if(this.$route.query.cid){
            localStorage['cid_one'] = this.$route.query.cid;
        }
        if(this.$route.query.pid){
            localStorage['pid_one'] = this.$route.query.pid;
        }
        
        this.pid = localStorage.getItem('pid_one');
        if(localStorage.getItem('pid_one')){
            this.has_pid = true;
        }else{
            this.pid = '512704';
        }
        let u = navigator.userAgent, app = navigator.appVersion;
        let isAndroid = u.indexOf('Android') > -1 || u.indexOf('Linux') > -1; //g
        let isIOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/); //ios终端
        if (isAndroid) {
            this.device = 0
        }
        if (isIOS) {
            this.device = 1
        }
    }
}
</script>

<style scoped lang="stylus">
/deep/ .van-field .van-cell__title
    max-width 1.6rem
    text-align left
.van_field
    width 76%
.van_field
    width 76%
.van_field_code
    /deep/ .van-field__control::-webkit-input-placeholder {
        color: #FC8D6D
    }
button 
    width 90%
    margin-top 1rem
.van_box
    border 1px solid #cccccc
    width 94%
    margin .3rem auto
    display flex
    align-items center
.container
    text-align center
</style>
