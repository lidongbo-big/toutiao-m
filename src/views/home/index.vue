<template>
  <div class="home-container">
    <van-nav-bar class="app-nav-bar">
      <van-button class="search-btn" slot="title" icon="search" type="info" round size="small">搜索</van-button>
    </van-nav-bar>
    <van-tabs class="channel-tabs" v-model="active">
      <van-tab :title="channel.name" v-for="channel in channels" :key="channel.id"> <article-list :channel="channel" /> </van-tab>
      <div slot="nav-right" class="wap-nav-placeholder"></div>
      <div @click="isChannelEditShow = true" class="wap-nav-wrap" slot="nav-right"><van-icon name="bars" /></div>
    </van-tabs>
    <van-popup
    v-model="isChannelEditShow"
    closeable
    position="bottom"
    class="channel-deit-popup"
    close-icon-position="top-left"
    get-container="body"
    style="height: 100%">
    <channel-edit :user-channels='channels' :active="active" @close="isChannelEditShow = false" @update-active="active = $event" />
  </van-popup>
  </div>
</template>

<script>
import { getUserChannels } from '@/api/user'
import ArticleList from './components/article-list'
import ChannelEdit from './components/channel-edit'
import { mapState } from 'vuex'
import { getItem } from '@/utils/storage'
export default {
  name: 'HomeIndex',
  components: {
    ArticleList,
    ChannelEdit
  },
  props: {},
  data () {
    return {
      active: 0,
      channels: [],
      isChannelEditShow: false // 控制编辑频道的显示状态
    }
  },
  computed: {
    ...mapState(['user'])
  },
  watch: {},
  created () {
    this.loadChannels()
  },
  mounted () {},
  methods: {
    async loadChannels () {
      let channels = []
      if (this.user) {
        const { data } = await getUserChannels()
        channels = data.data.channels
      } else {
        const localChannels = getItem('user-channels')
        if (localChannels) {
          channels = localChannels
        } else {
          const { data } = await getUserChannels()
          channels = data.data.channels
        }
      }
      this.channels = channels
      // const { data } = await getUserChannels()
      // this.channels = data.data.channels
      // console.log(data)
    }
  }
}
</script>

<style scoped lang="less">
.home-container {
 /deep/ .van-nav-bar__title {
    max-width: none;
  }
  .search-btn {
    width: 277px;
    height: 32px;
    background: #5babfb;
    border: none;
    .van-button__text {
      font-size: 14px;
    }
    .van-icon {
      font-size: 16px;
    }
  }
  .channel-tabs {
    /deep/ .van-tab {
      border-right: 1px solid #edeff3;
      border-bottom: 1px solid #edeff3;
    }
    /deep/ .van-tabs__line {
      bottom: 20px;
      width: 15px !important;
      height: 3px;
      background: #3296fa;
    }
  }
  .wap-nav-placeholder {
    width: 33px;
    flex-shrink: 0;
  }
  .wap-nav-wrap {
    position: fixed;
    right: 0;
    width: 33px;
    height: 43px;
    background-color: #fff;
    display: flex;
    justify-content: center;
    align-items: center;
    opacity: .9;
    .van-icon {
      font-size: 24px;
    }
    &:before {
      content: '';
      width: 1px;
      background: url("./line.png") no-repeat;
      background-size: contain;
      position: absolute;
      left: 0;
      top: 0;
      bottom: 0;
    }
  }
}
</style>
