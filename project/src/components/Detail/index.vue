<template>
    <div class="container">
        <title-bar :title_name="title" />
        <div>
            <!-- <h3 style="font-size:18px;text-align:center;padding:16px 0">{{title}}</h3> -->
            <div style="line-height:1.5;padding:.4rem .3rem;font-size:.38rem;" v-html="content">
                
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name:'detail',
    props: {
      title_name: {
        type: String,
        default: '详情'
      },
      title:{
        type: String,
        default: ''
      },
      content:{
        type: String,
        default: ''
      }
    },
}
</script>

<style>

</style>
