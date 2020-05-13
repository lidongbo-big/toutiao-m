<template>
  <div class="my-container">
   <van-cell-group v-if="user" class="cell-nav-group" :border='false'>
     <van-cell :border="false" class="cell-nav" center>
       <van-image class="avatar" slot="icon" round fit="cover" :src="currentUser.photo" />
       <div class="name" slot="title">{{ currentUser.name }}</div>
       <van-button class="van-button" size="small" round>编辑资料</van-button>
     </van-cell>
     <van-grid :border="false" class="van-grid">
       <van-grid-item><div class="text-wrap" slot="text"><div class="span">{{currentUser.art_count}}</div> <div class="text">头条</div> </div></van-grid-item>
       <van-grid-item><div class="text-wrap" slot="text"><div class="span">{{currentUser.follow_count}}</div> <div class="text">关注</div> </div></van-grid-item>
       <van-grid-item><div class="text-wrap" slot="text"><div class="span">{{currentUser.fans_count}}</div> <div class="text">粉丝</div> </div></van-grid-item>
       <van-grid-item><div class="text-wrap" slot="text"><div class="span">{{currentUser.like_count}}</div> <div class="text">获赞</div> </div></van-grid-item>
     </van-grid>
   </van-cell-group>
   <div class="not-login" v-else>
     <div>
       <img @click="$router.push('/login')" src="./head.png" class="mobile">
     </div>
     <div class="text">登录 / 注册</div>
   </div>
   <van-grid class="nav-grid-sc mb-4" :column-num="2">
     <van-grid-item class="nav-grid-item" icon-prefix="toutiao" icon="shoucang" text="收藏" />
     <van-grid-item class="nav-grid-item" icon-prefix="toutiao" icon="lishi" text="历史" />
   </van-grid>
    <van-cell title="消息通知" is-link to="/" />
    <van-cell class="mb-4" title="小智同学" is-link to="/" />
    <van-cell @click="onLogout" v-if="user" class="logout-cell" title="退出登录" />
   </div>
</template>

<script>
import { mapState } from 'vuex'
import { getCurrentUser } from '@/api/user'
export default {
  name: 'MyIndex',
  components: {},
  props: {},
  data () {
    return {
      currentUser: {}
    }
  },
  computed: {
    ...mapState(['user'])
  },
  watch: {},
  created () {
    this.loadCurrentUser()
  },
  mounted () {},
  methods: {
    async loadCurrentUser () {
      const { data } = await getCurrentUser()
      this.currentUser = data.data
    },
    onLogout () {
      this.$dialog.confirm({
        title: '推出提示',
        message: '确认推出？'
      })
        .then(() => {
          // on confirm
          this.$store.commit('setUser', null)
        })
        .catch(() => {
          // on cancel
        })
    }
  }
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
    height: 65px;
    color: #fff;
    .text-wrap {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
    .span {
      font-size: 18px;
    }
    .text {
      font-size: 12px;
    }
  }
 }
}
  /deep/ .nav-grid-sc {
    .nav-grid-item {
      height: 70px;
      .toutiao {
        font-size: 22px;
      }
      .toutiao-shoucang {
      color: #eb5253;
      }
      .toutiao-lishi {
        color: #ff9d1d;
      }
      .van-grid-item__text {
        font-size: 14px;
        color: #333;
      }
    }
  }
  .logout-cell {
    text-align: center;
    color: #d86262;
  }
  .mb-4 {
    margin-bottom: 4px;
  }
  .not-login {
    height: 180px;
    background: url("./banner.png") no-repeat;
    background-size: cover;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    .mobile {
      width: 66px;
      height: 66px;
    }
    .text {
      font-size: 14px;
      color: #fff;
    }
  }
</style>
