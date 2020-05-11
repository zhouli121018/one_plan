<template>
  <div >
    <div class="container" id="home_page">
      <!-- <van-pull-refresh v-model="isLoading" @refresh="onRefresh"> -->
        <div class="fixed_title">
          <van-nav-bar
            title="一个计划"
            :left-text="left_text"
            @click-left="onClickLeft"
          >
            <span slot="title" @click="gethome">一个计划
              <!-- <van-icon name="replay" /> -->
            </span>
            <div slot="right" v-if="lottype && lottype.length>0">
              <span class="top_right_title" @click="show_ul">
                {{lottype[active_lt].lotname}}
                <!-- <van-icon name="" /> -->
                <img src="~@/assets/home/down.png" style="width:.25rem;height:.15rem;position:relative;top:-0.06rem;"/>
              </span>
              <ul class="right_top_ul" v-show="show_lt">
                <li :class="{active:k== active_lt}" v-for="(l,k) in lottype" @click="change_lt(k)" :key="k">{{l.lotname}}</li>
              </ul>
            </div>
          </van-nav-bar>
        </div>
        <van-swipe :autoplay="3000" indicator-color="#007BC2">
          <van-swipe-item  v-for="(image, index) in advs" :key="index">
            <div class="swipe_img_box" @click="jumpTo(image.url)">
              <img :src="$https+image.pic" />
            </div>
          </van-swipe-item>
        </van-swipe>
        <a :href="banner_url" v-show="false" id="banner_a">1</a>
        
        <div class="flex" style="padding-top:.52rem;">
          <div class="flex_grow_1 text_center"  @click="jumpTo(l.link,l.islink)" v-for="(l,index) in new_links" :key="index">
            <img :src="l.src" :alt="l.title" style="width:.65rem;height:.64rem;">
            <div style="font-size:.37rem;padding:.2rem 0 .6rem;color:#383838;">{{l.title}}</div>
          </div>
        </div>

        <div class="middle_block" v-if="planInfo && lottype && lottype.length>0">
          <div class="blue_bg" style="box-shadow:0 0 .2rem #EEEEEE;">
            <div class="flex" style="color:#606266;">
              <div>{{lottype[active_lt].lotname}}</div> 
              <div class="flex_grow_1 text_right" >会员天数:{{planInfo.vipdays}} </div>
            </div>
            <div class="flex" style="color:#333333;padding:.4rem 0 .3rem;white-space:nowrap;">
              <div class="flex_grow_1">距{{planInfo.endissue}}期开奖:<span style="color:#E8541E;">{{kjdjs}}</span></div>
              <span>|</span>
              <div class="flex_grow_1 text_right">当前时间:{{curtime}}</div>
            </div>
            <div class="" style="color:#32373A;white-space:nowrap;">
              
              <div class="">{{planInfo.preissue}}期开奖:{{planInfo.prekjnum}}</div>
            </div>
          </div>

          <div style="border-radius:.1rem;margin-top:.3rem;font-size:0.37rem;color:#373737;box-shadow:0 0 .2rem #EEEEEE;white-space:nowrap;">
              <div class="flex" style="background:linear-gradient(-90deg,#E7FFFC,#fff);padding:.3rem .4rem;border-radius:.1rem; ">
                <span v-if="lottype && lottype.length>0 && lottype[active_lt].playtypes && lottype[active_lt].playtypes.length>0">玩法</span>
                <div style="position:relative;" v-if="lottype && lottype.length>0 && lottype[active_lt].playtypes && lottype[active_lt].playtypes.length>0">
                  <div style="border:1px solid #0BA194;padding:.1rem;margin:0 .2rem 0 .1rem;border-radius:.05rem;min-width:.9rem;" @click="click_pt">
                    <div class="flex text_center">
                      <span class="flex_grow_1" style="color:#0BA194;">{{lottype[active_lt].playtypes[active_pt].playname}}</span> 
                      <img src="~@/assets/home/down_blue.png" alt="1" style="width:.25rem;margin-left:.1rem;">
                    </div>
                  </div>
                  <ul v-show="show_pt" style="position:absolute;top:.7rem;left:.1rem;border:1px solid #0BA194;min-width:85%;background:#fff;box-shadow:0 0 .12rem #0BA194;max-height:5rem;overflow:auto;z-index:10;">
                    <li @click="change_pt(k)" class="check_li" v-for="(p,k) in lottype[active_lt].playtypes" :class="{active:active_pt==k}" :key="k">{{p.playname}}</li>
                  </ul>
                </div>
                

                <span v-if="lottype && lottype.length>0 && lottype[active_lt].playtypes && lottype[active_lt].playtypes.length>0 && lottype[active_lt].playtypes[active_pt].mashu">码数</span>
                <div style="position:relative;" v-if="lottype && lottype.length>0 && lottype[active_lt].playtypes && lottype[active_lt].playtypes.length>0 && lottype[active_lt].playtypes[active_pt].mashu">
                  <div style="border:1px solid #0BA194;padding:.1rem;margin:0 .2rem 0 .1rem;border-radius:.05rem;min-width:.9rem;" @click="click_ms">
                    <div class="flex text_center">
                      <span class="flex_grow_1" style="color:#0BA194;">{{lottype[active_lt].playtypes[active_pt].mashu.split(',')[active_mashu]}}</span> 
                      <img src="~@/assets/home/down_blue.png" alt="1" style="width:.25rem;margin-left:.1rem;">
                    </div>
                  </div>
                  <ul v-show="show_mashu" style="position:absolute;top:.7rem;left:.1rem;border:1px solid #0BA194;min-width:85%;background:#fff;box-shadow:0 0 .12rem #0BA194;max-height:5rem;overflow:auto;z-index:10;">
                    <li @click="change_ms(j)" class="check_li" v-for="(m,j) in lottype[active_lt].playtypes[active_pt].mashu.split(',')" :class="{active:active_mashu==j}" :key="j">{{m}}</li>
                  </ul>
                </div>
                

                <span v-if="lottype && lottype.length>0 && lottype[active_lt].playtypes && lottype[active_lt].playtypes.length>0 && lottype[active_lt].playtypes[active_pt].qishu">计划期数</span>
                <div style="position:relative;" v-if="lottype && lottype.length>0 && lottype[active_lt].playtypes && lottype[active_lt].playtypes.length>0 && lottype[active_lt].playtypes[active_pt].qishu">
                  <div style="border:1px solid #0BA194;padding:.1rem;margin:0 0 0 .1rem;border-radius:.05rem;min-width:.9rem;" @click="click_qs">
                    <div class="flex text_center">
                      <span class="flex_grow_1" style="color:#0BA194;">{{lottype[active_lt].playtypes[active_pt].qishu.split(',')[active_qishu]}}</span> 
                      <img src="~@/assets/home/down_blue.png" alt="1" style="width:.25rem;margin-left:.1rem;">
                    </div>
                  </div>
                  <ul v-show="show_qishu" style="position:absolute;top:.7rem;left:.1rem;border:1px solid #0BA194;min-width:85%;background:#fff;box-shadow:0 0 .12rem #0BA194;max-height:5rem;overflow:auto;z-index:10;">
                    <li @click="change_qs(j)" class="check_li" v-for="(m,j) in lottype[active_lt].playtypes[active_pt].qishu.split(',')" :class="{active:active_qishu==j}" :key="j">{{m}}</li>
                  </ul>

                </div>
                
              </div>
              <div class="flex text_center" style="">
                <div class=" flex_grow_1" style="padding:.3rem .1rem .3rem .4rem;" @click="click_check_plan">
                  <img src="~@/assets/home/refresh.png" alt="1" style="width:.45rem;">
                  <span>换一批计划</span>
                </div>
                <div class=" flex_grow_1" style="padding:.3rem .1rem;border-left:1px solid #EEEEEE;border-right:1px solid #EEEEEE;" @click="click_shoucang">
                  <img src="~@/assets/home/like_o.png" alt="1" style="width:.45rem;">
                  <span>收藏当前计划</span>
                </div>
                <div class=" flex_grow_1 text_right" style="padding:.3rem .4rem .3rem .1rem;" @click="goHisKj">
                  <img src="~@/assets/home/history.png" alt="1" style="width:.45rem;">
                  <span>历史开奖</span>
                </div>
              </div>
              <div style="padding:.2rem .4rem;color:#E8541E;border-top:1px solid #eeeeee;">{{planInfo.hitrate}}</div>
          </div>

          <div style="box-shadow:0 0 .2rem #EEEEEE;margin-top:.3rem;padding:.4rem 0 .7rem;font-size:.37rem; ">
            <table border="0" cellspacing="0" cellpadding="0">
              <tr>
                <th style="border-bottom:1px solid #eee;">计划期次</th>
                <th style="border-bottom:1px solid #eee;">计划内容</th>
                <th style="border-bottom:1px solid #eee;">计划结果</th>
              </tr>
              <tr v-for="(p,index) in planInfoList" :key="index">
                <td>{{p.issue}}</td>
                <td v-if="p.content == '会员续费查看'" style="color:#E7541E" @click="jumpTo('/personal/freeUse')">{{p.content}}</td>
                <td v-else>{{p.content}}</td>
                <td>{{p.result}}</td>
              </tr>
            </table>

            <div v-if="hasnextpage==1" @click="getplans" style="font-size:.37rem;color:#138EE6;text-align:center;margin:.28rem 0 0;">获取更多</div>
            <div v-if="planInfoList && planInfoList.length>0" class="text_center" style="padding-top:.48rem">
              <van-button size="large" style="background:#108FE9;color:#fff;width:90%;border-radius:.1rem;height:.83rem;line-height:.73rem;font-size:.37rem;" @click="show_tt = true">复制方案</van-button>
            </div>
          </div>

          

          <div class="card_item_box" v-if="false">
            <div class="item_title">积分查看提示</div>
            <div style="padding:.38rem 0;line-height:1.6;">
              查看计划将消耗你1积分，1积分将可以持续使用24小时。
            </div>
            <van-row gutter="20" style="">
              <van-col span="12">
                <van-button size="large" class="cancel_btn">取消</van-button>
              </van-col>
              <van-col span="12">
                <van-button size="large" class="sure_btn">使用</van-button>
              </van-col>
            </van-row>
          </div>

          
        </div>

        <van-dialog 
            v-model="show_tt"
            title="复制方案提示"
            show-cancel-button
            class="dialog_content_input"
            :before-close="beforeClose"
            >
            <van-field 
                v-model.trim.number="count_input"
                clearable
                label="最近条数："
            />
        </van-dialog>

        <van-dialog  v-if="planInfo && planInfo.user_plan_name"
            v-model="show_shoucang"
            title="收藏计划" 
            show-cancel-button
            class="dialog_content_input"
            :before-close="beforeClose_shoucang"
            >
            <div style="padding:.4rem .2rem;">计划名称：</div>
            <!--<div style="padding-bottom:.4rem;color:#108EE9;font-size:0.32rem;">
              {{user_plan_name}}
            </div> -->
            <van-field type="textarea" autosize
                v-model.trim="user_plan_name"
                clearable style="border:1px solid #eeeeee;"
                label=""
            />
        </van-dialog>

        <van-dialog  v-if="planInfo"
            v-model="show_change_plan"
            title="温馨提示" 
            show-cancel-button
            class="dialog_content_input"
            :before-close="beforeClose_change_plan"
            >
            <div style="padding:.4rem .2rem;line-height:1.6;">如果想回看当前计划，请收藏再切换。切换后无法找回，是否切换当前计划?</div>
        </van-dialog>

        <van-dialog  
            v-model="show_zhuce"
            title="登录提示" 
            show-cancel-button
            class="dialog_content_input"
            :before-close="beforeClose_go_login"
            >
            <div style="padding:.4rem .2rem;line-height:1.6;">您还未登录，请登录后再查看计划</div>
        </van-dialog>

        <van-dialog  
            v-model="show_go_free"
            title="温馨提示" 
            show-cancel-button
            class="dialog_content_input"
            :before-close="beforeClose_go_free"
            >
            <div style="padding:.4rem .2rem;line-height:1.6;">会员已过期，请免费获取会员天数。</div>
        </van-dialog>




      <!-- </van-pull-refresh> -->

    </div>

    <div class="container" v-if="is_ios" style="background:#F5F5F5;padding-top:0.4rem !important;position:absolute;top:0;z-index:10001;padding-bottom:2rem;">
        <!-- <title-bar title_name="添加到主屏幕" /> -->
        <div style="background:#EFEFEF;padding:0.2rem 0.15rem;margin:0 0.3rem 0.2rem;">
            <div style="text-align:center;font-size:0.5rem;color:#DB3030;font-weight:bold;padding:0.2rem 0;">温馨提示</div>
            <div style="line-height:1.6;padding-left:0.2rem;font-size:0.4rem;">
              请务必添加本页面到主屏幕，以便下次访问。
            </div>
            <div style="text-align:center;padding:0.4rem 0;">
              <van-button style="width:3rem;background-color:#D4D4D4;color:#1A1A1A;font-size:0.45rem;" @click="addfn">已添加</van-button>
              <van-button style="width:3rem;margin-left:1rem;background-color:#DB3030;color:#fff;font-size:0.45rem;" @click="ignore">关闭</van-button>
            </div>
        </div>
        <img src="../../assets/down_iphone.png" alt="" style="width:100%">
    </div>
    
  </div>
</template>

<script>
import { Dialog } from 'vant'
import { gethome } from '@/api/home'
import { getplan, like } from '@/api'
import { getdTime, getHMS } from '@/utils'
import Vue from 'vue'
import VueClipboard from 'vue-clipboard2'
Vue.use(VueClipboard)
export default {
  data () {
    return {
      show_tt:false,
      count_input: 20,
      show_lt:false,
      planInfo:null,
      curtime:'12',
      kjdjs:'',
      lastid:0,
      time_add:true,
      _curtime: '',
      planInfoList: [],
      endtime: '',
      current_time:'',
      isCurtime: false,
      h: '',
      m: '',
      s: '',
      lottype:[],
      active_lt:0,
      active_pt:0,
      mashu:0,
      qishu:0,
      show_pt: false,
      show_mashu: false,
      show_qishu: false,
      active_mashu:0,
      active_qishu:0,
      user_plan_id:0,
      user_plan_name:'',
      show_shoucang:false,
      show_change_plan: false,
      pList:[
        {issue:'12345',content:'会员续费查看',result:'追号中'},
        {issue:'12345',content:'会员查看',result:'追号中'},
        {issue:'12345',content:'1 2 5 8 6 ',result:'追号中'},
        ],
      hasnextpage:0,
      show_zhuce:false,
      show_go_free:false,


      list:[
        {src:require('../../assets/mfsy.png'),title:'免费使用',link:'/personal/freeUse',islink: false},
        // {src:require('../../assets/fajh.png'),title:'方案计划',link:'/home/aPlan',islink: false},
        {src:require('../../assets/kjtx.png'),title:'开奖提醒',link:'/home/openRemind',islink: false},
        {src:require('../../assets/gg.png'),title:'公告',link:'/home/announcement/index',islink: false},
        
        {src:require('../../assets/dlzq.png'),title:'推荐赚钱',link:'/home/earnMoney',islink: false},
        {src:require('../../assets/dlzq.png'),title:'推荐赚钱',link:'/home/earnMoney',islink: false},
        
      ],
      new_links:[
        {src:require('../../assets/home/zhuanjifen.png'),title:'永久免费',link:'/personal/freeUse'},
        {src:require('../../assets/home/vip.png'),title:'会员中心',link:'/personal/index'},
        {src:require('../../assets/home/gonggao.png'),title:'公告栏',link:'/home/announcement/index'},
        {src:require('../../assets/home/like.png'),title:'我的收藏',link:'/personal/like'},
        {src:require('../../assets/home/qunfa.png'),title:'群发计划',link:'/home/announcement/detail?noticeid=1'},
      ],
      notice:'',
      advs:[],
      left_text:'登录',
      left_path:'/login/index',
      banner_url:'#',
      is_ios:false,
      isFirstEnter:false,
      fangansList:null,
      notices:[],
      isLoading:false
    }
  },
  methods: {
    click_check_plan(){
      if(!localStorage.getItem('uid_one') || !localStorage.getItem('sid_one')){
        this.show_zhuce = true
      }else{
        this.show_change_plan = true;
      }
    },
    click_shoucang(){
      if(!localStorage.getItem('uid_one') || !localStorage.getItem('sid_one')){
        this.show_zhuce = true;
      }else{
        if(this.planInfo &&this.planInfoList.length==0){
          this.$toast('当前没计划收藏')
        }else{
          this.show_shoucang = true;
        }
      }
    },
    beforeClose_go_free(action,done){
      if(action == 'confirm'){
        this.$router.push('/personal/freeUse');
      }
      done();
    },
    beforeClose_go_login(action,done){
      if(action == 'confirm'){
        this.$router.push('/login/index');
      }
      done();
    },
    beforeClose_change_plan(action,done){
      if(action == 'confirm'){
        this.user_plan_id = 0;
        this.lastid = 0;
        this.getplans();
      }
      done();
    },
    goHisKj(){
      this.$router.push({
        path:'/home/hisKj',
        query:{
          lottype: this.lottype[this.active_lt].lottype,
          lotname: this.lottype[this.active_lt].lotname
        }
      })
    },
    beforeClose_shoucang(action,done){
      if(action == 'confirm'){
        if(!this.user_plan_name){
            this.$toast('请输入计划名称！')
            done(false)
            return;
        }
        this.like();
      }
      done();
    },
    async like () {
      const { data }    = await like({
            user_plan_id: this.user_plan_id,
            planname: this.user_plan_name
        });
    },
    beforeClose(action,done){
        if(action == 'confirm'){
            if(!this.count_input){
                this.$toast('请输入要复制的最近条数！')
                done(false)
                return;
            }
            this.doCopy(this.planInfoList);
            this.count_input = 20
        }
        done();
    },
    //复制
    doCopy (text) {
        let arr = []
        // let txt = text.map(item => {
        //     arr.push(`${item.issue}  ${item.content}  ${item.hitnum}  `)
        // })
        let len = this.count_input;
        if(len > text.length){
            len = text.length;
        }
        for(let i = 0;i<len;i++){
            let item = text[i]
            arr.push(`${item.issue}  ${item.content}  ${item.result}  `)
        }
        arr = `--------------------------- \n ${arr.join('\n')} \n--------------------------- ` 
        this.$copyText(arr).then( (e) => {
            Dialog.alert({
                title: '提示',
                message: '复制成功，请粘贴分享到微信或QQ。'
            }).then(() => {
            // on close
            });
        },  (e) => {
            Dialog.alert({
                title: '提示',
                message: '复制失败，请手动复制！'
            }).then(() => {
            // on close
            });
            console.log(e)
        })
    },
    show_ul(){
      this.show_mashu = false;
      this.show_qishu = false;
      this.show_pt = false;
      this.show_lt = !this.show_lt;
    },
    click_pt(){
        this.show_lt = false;
        this.show_mashu = false;
        this.show_qishu = false;
        this.show_pt = !this.show_pt;
    },
    click_ms(){
        this.show_lt = false;
        this.show_qishu = false;
        this.show_pt = false;
        this.show_mashu = !this.show_mashu;
    },
    click_qs(){
        this.show_lt = false;
        this.show_mashu = false;
        this.show_pt = false;
        this.show_qishu = !this.show_qishu;
    },
    change_lt(index){
      this.show_lt = false;
      this.lastid = 0;
      this.user_plan_id = 0;
      this.active_mashu = 0;
      this.active_qishu = 0;
      this.active_pt = 0
      this.active_lt = index
      this.getplans();
    },
    change_pt(index){
        this.show_pt = false;
        this.active_mashu = 0;
        this.active_qishu = 0;
        this.active_pt = index
        this.lastid = 0;
        this.user_plan_id = 0;
        this.getplans();
    },
    change_ms(index){
        this.show_mashu = false;
        this.active_mashu = index;
        this.lastid = 0;
        this.user_plan_id = 0;
        this.getplans();
    },
    change_qs(index){
        this.show_qishu = false;
        this.active_qishu = index;
        this.lastid = 0;
        this.user_plan_id = 0;
        this.getplans();
    },
    async getplans() {
        if(this.$store.getters.timer) {
            clearTimeout(this.$store.getters.timer)
            this.$store.dispatch('set_timer',null)
        }
        if(this.$store.getters.cur_timer){
            clearTimeout(this.$store.getters.cur_timer)
            this.$store.dispatch('set_cur_timer',null)
        }
        let obj = {
          lottype: this.lottype[this.active_lt].lottype,
          playtype: this.lottype[this.active_lt].playtypes[this.active_pt].playtype,
          pos_type: this.lottype[this.active_lt].playtypes[this.active_pt].pos_type,
          mashu: this.lottype[this.active_lt].playtypes[this.active_pt].mashu.split(',')[this.active_mashu],
          qishu: this.lottype[this.active_lt].playtypes[this.active_pt].qishu.split(',')[this.active_qishu],
          lastid: this.lastid,
          user_plan_id: this.user_plan_id
        }
        const { data } = await getplan(obj)
        
        this.planInfo = data;
        this.user_plan_id = data.user_plan_id;
        this.user_plan_name = data.user_plan_name;
        this.hasnextpage = data.hasnextpage;
        

        localStorage.setItem('lottype_one',obj.lottype);
        localStorage.setItem('playtype_one',obj.playtype);
        localStorage.setItem('pos_type_one',obj.pos_type);
        localStorage.setItem('mashu_one',obj.mashu);
        localStorage.setItem('qishu_one',obj.qishu);
        localStorage.setItem('user_plan_id_one',data.user_plan_id);
        localStorage.setItem('playname_one',this.lottype[this.active_lt].playtypes[this.active_pt].playname);

        

        let planInfoList = data.list;
        if(this.lastid != 0 && this.time_add) {
            planInfoList = planInfoList.map(item => {
                this.planInfoList.push(item)
            })
        }else {
            this.planInfoList = planInfoList
        }  
        this.time_add = true; 
        this.lastid = this.planInfo.lastid  //获取更多传当前这个lastid 默认传0

        this.planInfo.curtime = this.planInfo.sertime;
        this.curtime = getHMS(this.planInfo.curtime)//开始时间
        this._curtime = this.planInfo.curtime*1000//当前时间
        this.endtime = this.planInfo.endtime*1000//结束时间
        this.current_time = this.planInfo.curtime*1000//当前时间
        this.isCurtime = false
        this.curTime();
        this.countTime()

        if(this.$store.getters.kj_number_timer){
            clearTimeout(this.$store.getters.kj_number_timer)
            this.$store.dispatch('set_kj_number_timer',null)
        }
        
        if(data.prekjnum == '开奖中...'){
            this.lastid = 0;
            this.$store.dispatch('set_kj_number_timer',setTimeout(()=>{
              if(this.$store.getters.timer) {
                  clearTimeout(this.$store.getters.timer)
                  this.$store.dispatch('set_timer',null)
              }
              if(this.$store.getters.cur_timer){
                  clearTimeout(this.$store.getters.cur_timer)
                  this.$store.dispatch('set_cur_timer',null)
              }
              this.getplans();
            },3000))
            return;
        }

        
    },
    countTime () {
        //时间差
        let leftTime = this.endtime - this._curtime;
        //定义变量 d,h,m,s保存倒计时的时间
        if (leftTime >= 0) {
            if(leftTime == 0){
                this.time_add = false;
                this.lastid = 0
                this.getplans();
                if(this.$store.getters.timer) {
                  clearTimeout(this.$store.getters.timer)
                  this.$store.dispatch('set_timer',null)
                }
                return;
            }
            this._curtime = this._curtime + 1000
            // this.d = Math.floor(leftTime / 1000 / 60 / 60 / 24);
            this.h = Math.floor(leftTime / 1000 / 60 / 60 % 24)>=10?Math.floor(leftTime / 1000 / 60 / 60 % 24):'0'+Math.floor(leftTime / 1000 / 60 / 60 % 24);
            this.m = Math.floor(leftTime / 1000 / 60 % 60)>=10?Math.floor(leftTime / 1000 / 60 % 60):'0'+Math.floor(leftTime / 1000 / 60 % 60);
            this.s = Math.floor(leftTime / 1000 % 60)>=10?Math.floor(leftTime / 1000 % 60):'0'+Math.floor(leftTime / 1000 % 60);
            this.kjdjs = this.h+':'+this.m+':'+this.s
            //递归每秒调用countTime方法，显示动态时间效果
            this.$store.dispatch('set_timer',setTimeout(this.countTime, 1000))
        }else {
            if(this.$store.getters.timer) {
                clearTimeout(this.$store.getters.timer)
                this.$store.dispatch('set_timer',null)
            }
        }
    },
    curTime () {
        this.current_time = this.current_time + 1000
        var date=new Date(this.current_time);
        var year=date.getFullYear();
        var month=date.getMonth()+1;
        var day=date.getDate();
        var hour="00"+date.getHours();
        hour=hour.substr(hour.length-2);
        var minute="00"+date.getMinutes();
        minute=minute.substr(minute.length-2);
        var second="00"+date.getSeconds();
        second=second.substr(second.length-2);
        // let str = year+"-"+month+"-"+day+" "+" "+hour+":"+minute+":"+second
        let str = hour+":"+minute+":"+second
        this.curtime = str
        if(this.$store.getters.cur_timer){
            clearTimeout(this.$store.getters.cur_timer)
            this.$store.dispatch('set_cur_timer',null)
        }
        this.$store.dispatch('set_cur_timer',setTimeout(this.curTime, 1000))
    },
    onRefresh() {
      this.pull_refresh();
    },
    goDetail(data){
        this.$router.push({
            path: '/home/announcement/detail', 
            query: {
                // title: data.title, 
                noticeid: data.noticeid
            }
        })
    },
    addfn(){
      this.is_ios = false;
    },
    ignore(){
      this.is_ios = false;
    },
    onClickLeft() {
      this.$router.push(this.left_path)
    },
    jumpTo( path, islink ){
      if(path.indexOf('/')==0){
        this.$router.push(path)
      }else{
        this.banner_url = path;
        this.$nextTick(()=>{
          document.getElementById('banner_a').click();
        })
      }
    },
    goSinglePlan(p){
      this.$router.push({
        path:'/home/singlePlan',
        query:{
          fanganid:p.fanganid
        }
      })
    },
    async gethome() {
      let obj = {};
      const { data } = await gethome(obj)
      this.isLoading = false;
      this.advs = data.advs 
      this.$store.dispatch('set_homedata',data)
      this.$store.dispatch('set_kfwecha',data.kfwecha)
      this.$store.dispatch('set_issetkjtx',data.issetkjtx)
      this.$store.dispatch('set_apkurl',data.apkurl)
      this.lottype = data.list;
      if(data.errorcode == 101){
        this.show_zhuce = true;
      }else if(data.errorcode == 102){
        this.show_go_free = true;
      }else{
        this.show_zhuce = false;
        this.show_go_free = false
      }
    },
    pull_refresh(){
        this.$router.go(0)
    },
  },
  created(){
    this.isFirstEnter=true;
    //判断 浏览器类型
    if (/(iPhone|iPad|iPod|iOS)/i.test(navigator.userAgent)) {
      
      
      if(window.navigator.standalone){
        this.is_ios = false;
      }else{
        this.is_ios = true;
      }
    }

    if(this.$route.query.cid){
      localStorage['cid_one'] = this.$route.query.cid;
    }
    if(this.$route.query.pid){
      localStorage['pid_one'] = this.$route.query.pid;
    }
    
    document.addEventListener("visibilitychange", () => {
        if (document.hidden) {
          
        } else {
          if(this.$route.name == 'home'){
            
          }
        }
    })
  },
  activated(){  
    this.show_lt = false;
    this.show_mashu = false;
    this.show_pt = false;
    this.show_qishu = false;

    

    if(localStorage['uid_one'] && localStorage['uid_one']!=''){
      this.left_text = '会员中心';
      this.left_path = '/personal/index'
    }else{
      this.left_text = '登录';
      this.left_path = '/login/index'
    }
    if(localStorage.getItem('user_plan_id_one') && localStorage.getItem('user_plan_id_one')>0){
      this.user_plan_id = localStorage.getItem('user_plan_id_one');
    }
    if(!this.$store.getters.isback || this.isFirstEnter){
      this.gethome().then(()=>{
        if(localStorage.getItem('lottype_one') && localStorage.getItem('lottype_one')>0){
          for(var i = 0;i<this.lottype.length;i++){
            if(localStorage.getItem('lottype_one') == this.lottype[i].lottype){
              this.active_lt = i;
            }
          }
        }
        if(localStorage.getItem('pos_type_one') && localStorage.getItem('playtype_one')){
          let active_item = this.lottype[this.active_lt].playtypes;
          for(var i=0;i<active_item.length;i++){
            if(active_item[i].playtype==localStorage.getItem('playtype_one') && active_item[i].pos_type==localStorage.getItem('pos_type_one')){
              this.active_pt = i;
              let arr_mashu = active_item[i].mashu.split(',')
              for(let k=0;k<arr_mashu.length;k++){
                if(arr_mashu[k] == localStorage.getItem('mashu_one')){
                  this.active_mashu = k;
                }
              }
              let arr_qishu = active_item[i].qishu.split(',')
              for(let k=0;k<arr_qishu.length;k++){
                if(arr_qishu[k] == localStorage.getItem('qishu_one')){
                  this.active_qishu = k;
                }
              }
            }
          }
        }
        this.getplans();
      })
    }else{
      this.lastid = 0;
      this.getplans();
      
    }
    
    
    this.isFirstEnter=false;
    this.$store.dispatch('set_isback',false)
    
    if(localStorage['uid_one'] && localStorage['uid_one']!=''){
      this.left_text = '会员中心';
      this.left_path = '/personal/index'
    }
  },
  beforeRouteLeave (to, from, next) {
      // 导航离开该组件的对应路由时调用
      // 可以访问组件实例 `this`
      if(this.$store.getters.timer) {
          clearTimeout(this.$store.getters.timer)
          this.$store.dispatch('set_timer',null)
      }
      if(this.$store.getters.cur_timer){
          clearTimeout(this.$store.getters.cur_timer)
          this.$store.dispatch('set_cur_timer',null)
      }
      if(this.$store.getters.kj_number_timer){
          clearTimeout(this.$store.getters.kj_number_timer)
          this.$store.dispatch('set_kj_number_timer',null)
      }
      next();
  }
}
</script>

<style scoped lang="stylus">
.check_li
  border-bottom 1px solid #0BA194;
  font-size .32rem
  padding .2rem .1rem
  &.active
    background #0BA194
    color #fff
.card_item_box
  box-shadow:0 0 .2rem #EEEEEE;
  margin-top:.4rem;
  padding:.4rem .4rem .48rem;
  font-size:.37rem;
  .item_title
    padding-bottom:.4rem;
    border-bottom:1px solid #eee;
    text-align:center;
    font-size:0.4rem;
    font-weight:bold;
  .cancel_btn,.sure_btn
    border-radius:0.02rem;
    height:.83rem;
    line-height:.73rem;
    font-size:0.37rem;
  .cancel_btn
    background:#EEEEEE;
    color:#333333;
  .sure_btn
    background:#189BFF;
    color:#ffffff;
.middle_block
  padding:.43rem;
  font-size:.37rem;
  div.blue_bg
    background:#EFF7FF;
    box-shadow:0 0  .1rem #EFF7FF;
    border-radius:.1rem;
    padding:.4rem;
.right_top_ul
  background #fff
  position absolute
  top 1rem
  right -0.2rem
  box-shadow 0 0 .1rem #aaa
  line-height 2
  >li.active
    color #ff5c0a
  >li
    padding .1rem .2rem
    text-align left
    white-space nowrap
.top_right_title
  color #666
  .van-icon
    color #666

.green
  color #2fc900
.blue
  color #0096ff
/deep/ .membership_privileges
    border:none;
    background:#FFBD28!important
    color:#FF0B60;
    height:.6rem;
    line-height:.4rem
    font-size .32rem
table 
    font-size .32rem
    width 100%
    border-right none
    tr 
        border-right none
    th   
        color #A8A8A8
        padding-bottom .4rem
        color #373737
    td 
        padding .2rem 0
        text-align center
        color #373737
    td  
        color #2B2B2B
        font-size .35rem
    th
        font-size .37rem

button.active_color{
  background:#fc7953;
  border-color:#fc7953;
  color:#fff;
}
.msg_box .van-cell{
  padding:0.12rem 0;
}
.msg_box .msg_item .van-cell__title span{
  color:#363636;
  /* font-size:0.4rem; */
}
.van-cell:not(:last-child)::after{
  content:"";
  border:none;
}
.van-cell__value{
  flex:unset;
  padding-left:0.1rem;
}
.diy_font.van-cell{
  padding-bottom:0.2rem;
}
.diy_font .van-cell__right-icon{
  font-size:0.3rem;
}
.van-cell .van-cell__title span{
  /* font-size:0.4rem; */
  color:#7D7D7D;
}
.van-cell__value span{
  font-size:0.3rem;
}
.van-cell{
  background:none;
  padding:0;
  padding-bottom:0.1rem;
}
/deep/ .van-dialog__content .van-cell{
  padding: 10px 15px;
}
.left_border_ori{
  /* border-left:0.1rem solid #FC7953; */
  display:inline-block;
  /* padding:0; */
  /* padding-left:0.2rem; */
  /* font-size:0.4rem; */
  /* color:#7D7D7D; */
  background:#FC7953;
  width:0.08rem;
  height:0.4rem;
  position:relative;
  top:0.08rem;
  margin-right:0.1rem;
}
.max_width_100{
  margin-bottom: .15rem;
  width:1.4rem;
  height: 1.4rem;
}
  .btn_group button{
    margin-top:.1rem;
  }
  .btn_group{
    padding-bottom:.1rem;
  }
  select{
    outline: none;
  }
  .red{
    color:red;
  }
  .no_border{
    border:none;
  }
  .text_box{
    padding:20px 4px;
  }
  .text_center{
    text-align:center;
  }
  /* #home_page .van-nav-bar__title{ */
    /* font-size:0.5rem; */
  /* } */
  #home_page .van-nav-bar__left .van-nav-bar__text{
    color:#138EE6;
  }
  #home_page .van-nav-bar__right .van-nav-bar__text{
    color:#2C2C2C;
  }
  .swipe_img_box{
    width:100%;
    padding:0 12px;
    box-sizing: border-box;
  }
  #home_page .van-swipe-item img{
    width:100%;
    border-radius:.2rem;
  }
  .grow_1 .van-notice-bar{
    padding:0;
    background:none !important;
  }
  #home_page .tabs_type .van-tab{
    color:#1D1D1D;
    padding:0;
  }
  #home_page .tabs_type .van-tab>span{
    font-weight:bold;
  }
  #home_page .tabs_type .van-tab--active,#home_page .van-tab--active{
    color:#E55546;
  }
  .fixed_title{
    position: fixed;
    width: 100%;
    left: 0;
    right: 0;
    z-index: 1000;
    top: 0;
  }
  
</style>
