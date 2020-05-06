<template>
    <detail :title_name="title_name"  :title="title" :content="content"/>
</template>

<script>
import {getnotice } from '@/api'
export default {
    data(){
        return {
            title_name: '关于',
            title:'',
            content:''
        }
    },
    methods:{
        async getnotice () {
          const { data }    = await getnotice({
              noticeid: 2
          });
          if(data.errorcode==0){
            this.title = data.title;
            this.content = data.content;
          }
        },
    },
    created(){
        this.getnotice();
    }
}
</script>

<style>

</style>
