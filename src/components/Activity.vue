<template>
  <div class="activity">
      <div class="price">
         <ol id="myTab" class="list-inline" style="width:100%;">
              <li v-for="(tab,index) in tabs" >
                <a @click="toggle(index,tab.view)" :class="{active:active == index}">
                  <img v-bind:src="tab.image"  alt="" />{{tab.title}}
                </a>
              </li>
          </ol>
      </div>
      <div id="myTabContent" class="tab-content">
         <component :is="currentView"></component>
      </div>
  </div>
</template>

<script>

import Productsintro from '@/pages/productsintro'

import Download from '@/pages/download'

import Teamlive from '@/pages/teamlive'

import Activityzone from '@/pages/activityzone'

import API from '@/api/API'
//实例化api
const api = new API();

import { mapGetters } from 'vuex'

export default {
  name: 'activity',
  data () {
    return {
      active: 0,
      currentView: 'Activityzone',
      /*
      tabs: [
         {
          view: 'Productsintro',
          image:'../../static/images/product-icon.png',
          title:'产品介绍'
         },
         {
          view: 'Download',
          image:'../../static/images/xiazai.png',
          title:'下载中心'
         },
         {
          view: 'Teamlive',
          image:'../../static/images/jinrong-icon.png',
          title:'战队直播'
         },
         {
          view: 'Activityzone',
          image:'../../static/images/zhibo-icon.png',
          title:'活动专区'
         }
       ]
       */
       tabs: [
       {
          view: 'Activityzone',
          image:'../../static/images/zhibo-icon.png',
          title:'活动专区'
         },
         {
          view: 'Teamlive',
          image:'../../static/images/jinrong-icon.png',
          title:'战队直播'
         },
         {
          view: 'Download',
          image:'../../static/images/xiazai.png',
          title:'下载中心'
         }
       ]
    }
  },
  components:{ Productsintro, Download, Teamlive, Activityzone },
  methods: {
    toggle(i, v){
       if(v == 'Teamlive'){
          if(window.localStorage.getItem("clf-user")){
              let templateLevel = JSON.parse(window.localStorage.getItem("clf-user")).Level;
              if(parseInt(templateLevel)>=1){
                this.active = i;
                this.currentView = v;
                this.$store.dispatch('changeLive',1);
              }else{
                alert("普通用户暂不可以观看战队直播！");
              }
          }else{
            alert("未登录，不可以进入战队直播！");
          }
       }else{
         this.active = i;
         this.currentView = v;
       }
    }
  }
}
</script>

<style scoped>
  .activity .price li{
    width:33%;
    line-height:80px;
    text-align:center;
    font-size:20px;
    cursor:pointer;
  }

  .activity{
    width:100%;
    height:auto;
    overflow:hidden;
  }

  .activity .price li a>img{
    margin-right:10px;
  }
</style>
