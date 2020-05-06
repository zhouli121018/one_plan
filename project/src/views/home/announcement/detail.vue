<template>
    <detail :title_name="title_name"  :title="title" :content="content"/>
    
</template>

<script>
import {getnotice } from '@/api'
export default {
    data(){
        return {
            title_name: '公告详情',
            title:'',
            content:''
        }
    },
    methods:{
        async getnotice () {
          const { data }    = await getnotice({
              noticeid:this.$route.query.noticeid
          });
          if(data.errorcode==0){
              this.title = data.title;
            this.content = data.content;
          }
        },
    },
    created(){
        // this.title_name = this.$route.query.title
        this.getnotice();
    }
}
</script>

<style>

</style>
