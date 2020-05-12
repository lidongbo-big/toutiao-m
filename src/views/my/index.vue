<template>
  <div class="my-container">
   <van-cell-group  class="cell-nav-group" border='false'>
     <van-cell :border="false" class="cell-nav" center title="单元格" value="内容">
       <van-image class="avatar" slot="icon" round fit="cover" src="https://img.yzcdn.cn/vant/cat.jpeg" />
       <div class="name" slot="title">昵称</div>
       <van-button class="van-button" size="small" round>编辑资料</van-button>
     </van-cell>
     <van-grid :border="false" class="van-grid">
       <van-grid-item><div class="span">12</div> <div class="text">头条</div> </van-grid-item>
       <van-grid-item><div class="span">12</div> <div class="text">关注</div> </van-grid-item>
       <van-grid-item><div class="span">12</div> <div class="text">粉丝</div> </van-grid-item>
       <van-grid-item><div class="span">12</div> <div class="text">获赞</div> </van-grid-item>
     </van-grid>
   </van-cell-group>
   <van-grid :column-num="2">
     <van-grid-item icon-prefix="toutiao" icon="shoucang" text="收藏" />
     <van-grid-item icon-prefix="toutiao" icon="lishi" text="历史" />
   </van-grid>
    <van-cell title="消息通知" is-link to="/" />
    <van-cell title="小智同学" is-link to="/" />
    <van-cell title="推出登录" />
   </div>
</template>

<script>
export default {
  name: 'MyIndex',
  components: {},
  props: {},
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style scoped lang="less">
.cell-nav-group {
  background: url("./banner.png") no-repeat;
  .cell-nav {
    padding: 38px auto 11px;
    box-sizing: border-box;
    height: 115px;
    background-color: unset;
    .avatar {
      width: 66px;
      height: 66px;
      border: 1px solid #fff;
      box-sizing: border-box;
      margin-right: 11px;
    }
    .name {
      color: #fff;
      font-size: 15px;
    }
    .van-button {
    width: 57px;
    height: 16px;
    font-size: 10px;
    color: #666;
  }
  }
  /deep/.van-grid-item__content{
      background-color: unset;
  }
  .van-grid {
    .span {
      font-size: 18px;
    }
    .text {
      font-size: 12px;
    }
  }
}
</style>
