<template>
    <div class="container">
        <title-bar title_name="密码登录" right_text="注册" right_url="/register/index"/>
        <div class="van_box">
            <van-field label="帐号"  clearable v-model="mobile" placeholder="请输入帐号" />
        </div>
        <div class="van_box">
            <van-field label="密码"  type="number" maxlength="11"   class="van_field" clearable v-model="code" placeholder="请输入密码" />
            
        </div>
        <!-- <router-link tag="div" to="/login/verification" class="van_box_right">验证码登录</router-link> -->
        <van-button  style="background:#108FE9;color:#fff;border-radius:.1rem;"  @click="loginbypass">登录</van-button>
        <router-link tag="div" to="/register/index" class="van_box_right">注册</router-link>
    </div>
</template>

<script>
import { loginbypass } from '@/api/index'
export default {
    data() {
        return {
            mobile: '',
            code: ''
        }
    },
    methods: {
        async loginbypass () {
            if(!this.mobile || !this.code){
                this.$toast('请填写帐号和密码！');
                return;
            }
            const { data }    = await loginbypass({
                phone: this.mobile,
                pass: this.code
            });
            if(data.errorcode == 0) {
                window.localStorage['uid_one'] = data.uid
                window.localStorage['sid_one'] = data.sid
                this.$router.replace('/home/index')
            }
            
        },
    },
}
</script>

<style scoped lang="stylus">
/deep/ .van-field .van-cell__title
    max-width 1.6rem
    text-align left
button 
    width 90%
    margin-top .5rem
.van_box
    border 1px solid #cccccc
    width 94%
    margin .3rem auto
    display flex
    align-items center
.container
    text-align center
.van_box_right
    text-align right
    float right
    padding .3rem
    box-sizing border-box
    color #14ADFF
    padding-right .8rem
</style>
