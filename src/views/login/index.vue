<template>
  <div class="login-container">
    <van-nav-bar
    class="app-nav-bar"
    title="登录/注册"
    left-arrow
    @click-left="$router.back()"
    />
    <!-- 登录框 -->
    <van-cell-group>
    <van-field
        v-model="user.mobile"
        icon-prefix="toutiao"
        left-icon="shouji"
        placeholder="请输入手机号"
    />
    <van-field
        v-model="user.code"
        clearable
        icon-prefix="toutiao"
        left-icon="yanzhengma"
        placeholder="请输入验证码"
    >
    <template #button>
     <van-button class="send-btn" size="small" round>发送验证码</van-button>
    </template>
    </van-field>
    </van-cell-group>
    <div class="login-btn-wrap">
        <van-button @click="onLogin" class="login-btn" type="info" block>登录</van-button>
    </div>
  </div>
</template>

<script>
import { login } from '@/api/user'
export default {
  name: 'LoginIndex',
  components: {},
  props: {},
  data () {
    return {
      user: {
        mobile: '',
        code: ''
      }
    }
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {
    async onLogin () {
      try {
        const res = await login(this.user)
        console.log(res)
      } catch (err) {
        console.log(err)
      }
    }
  }
}
</script>

<style scoped lang="less">
.login-container {
    .send-btn {
        width: 80px;
        height: 26px;
        background-color: #ededed;
        .van-buttob_text {
            font-size: 11px;
            color: #666;
        }
    }
    .login-btn-wrap {
        padding: 26px 16px;
        .login-btn {
            color: #6db4fb;
            border: none;
            .van-button__text {
                font-size: 11px;
                color: #fff;
            }
        }
    }
}
</style>
