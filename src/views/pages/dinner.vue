<!--
 * @Author: 周祥毅
 * @Date: 2020-03-06 17:22:30
 * @LastEditors: 周祥毅
 * @LastEditTime: 2020-03-06 17:23:28
 * @Description: 餐饮组件
 -->
<template>
  <div class="dinner">
    <div class="content">
      <div class="content-left">
        <!-- 轮播图 -->
        <el-carousel :interval="4000" height="250px">
          <el-carousel-item v-for="(item,index) in photos" :key="index">
              <img :src="item.url" alt="" class="imgs">
          </el-carousel-item>
        </el-carousel>
      </div>
      <div class="content-right">
        <!-- 基本信息 -->
        <div class="baseinfo">
           <ul>
             <li class="name">{{baseinfo.p_name}}</li>
             <li class="address">地址:{{baseinfo.address}}</li>
             <li class="type">类型:{{baseinfo.p_type}}</li>
             <li class="cost">花费: <span>{{baseinfo.biz_ext.cost|costformat}}</span></li>
              <li class="rating"><span>评分:</span>
                <el-rate
                  v-model="baseinfo.biz_ext.rating"
                  disabled
                  show-score
                  text-color="#ff9900"
                  >
                </el-rate>
              </li>
               <li class="tel">电话: <span>{{baseinfo.tel}}</span></li>
                <li class="tel">距离: <span>{{baseinfo.distance}}米</span></li>
           </ul>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "Dinner",
  data() {
    return {
      baseinfo: this.pointinfo,
      panelid: this.layerid,
      photos:[{
        url:require('@/assets/default.png')
      }]
    };
  },
  props: {
    pointinfo: {
      type: Object,
      default: () => {
        return {};
      }
    },
    layerid: {
      //自动注入的layerid
      type: String,
      default: ""
    }
  },
  created(){
    if(this.baseinfo.photos.length>0){
      this.photos = this.baseinfo.photos
    }
    //处理评分为数字
    if(this.baseinfo.biz_ext.rating instanceof Array){
       this.baseinfo.biz_ext.rating = 0
    }else{
      this.baseinfo.biz_ext.rating = Number(this.baseinfo.biz_ext.rating)
    }
    //处理电话
    if(this.baseinfo.tel instanceof Array){
       this.baseinfo.tel = '-'
    }
  },
  mounted() {
    
    this.test();
    //将弹窗id提交到store
    this.$store.commit("updatepanelid", this.panelid);
  },
  methods: {
    test() {
      console.log(this.pointinfo);
      console.log(88888);
    }
  },
  filters:{
    costformat(cost){
        if(cost instanceof Array){
          return '-'
        }else{
          return `¥ ${cost}`
        }
    }
  }
};
</script>

<style lang="less" scoped>
  @import '~styles/mixins.less';
  @import '~styles/variable.less';
.dinner{
  width: 100%;
  padding: 10px;
  .content{
    display: flex;
    .content-left{
      margin-top: 14px;
      margin-right:12px ;
      flex: 1.5;
      img{
         height:100%;
         width:100%;
      }
    }
    .content-right{
      flex: 1;
      ul{
        li{
          color: @color-content;
          font-size: 14px;
          margin-bottom: 13px;
        }
        .name{
          color: @color-background;
          font-weight: bold;
          font-size: 18px;

        }
        .rating{
          display: flex;

        }
        .address{
          margin-top:10px ;
        }
      }
    }
  }
}
</style>
 
