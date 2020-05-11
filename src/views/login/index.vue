<template>
  <div class="login-container">
    <van-nav-bar
    class="app-nav-bar"
    title="登录/注册"
    left-arrow
    @click-left="$router.back()"
    />
    <!-- 登录框 -->
    <van-form @submit="onLogin">
    <van-field
        v-model="user.mobile"
        icon-prefix="toutiao"
        left-icon="shouji"
        placeholder="请输入手机号"
        :rules="fromRules.mobile"
    />
    <van-field
        v-model="user.code"
        clearable
        icon-prefix="toutiao"
        left-icon="yanzhengma"
        placeholder="请输入验证码"
        :rules="fromRules.code"
    >
    <template #button>
     <van-button class="send-btn" size="small" round>发送验证码</van-button>
    </template>
    </van-field>
      <div class="login-btn-wrap">
        <van-button class="login-btn" type="info" block>登录</van-button>
      </div>
    </van-form>
  </div>
</template>

<script>
import { login } from '@/api/user'
import { Toast } from 'vant'
export default {
  name: 'LoginIndex',
  components: {},
  props: {},
  data () {
    return {
      user: {
        mobile: '',
        code: ''
      },
      fromRules: {
        mobile: [
          { required: true, message: '请输入手机号' },
          { pattern: /^1[3|4|5|7|8|9]\d{9}$/, message: '请输入正确的手机号' }
        ],
        code: [
          { required: true, message: '请输入验证码' },
          { pattern: /^\d{6}$/, message: '请输入正确验证码' }
        ]
      }
    }
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {
    async onLogin () {
      Toast.loading({
        message: '登录中...',
        forbidClick: true,
        duration: 0
      })
      try {
        const res = await login(this.user)
        Toast.success('登录成功')
        console.log(res)
      } catch (err) {
        console.log(err)
        Toast.fail('登录失败，手机号或验证码错误')
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
