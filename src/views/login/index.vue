<template>
  <div class="login-container">
    <van-nav-bar
    class="app-nav-bar"
    title="登录/注册"
    left-arrow
    @click-left="$router.back()"
    />
    <!-- 登录框 -->
    <van-form ref="login-form" @submit="onLogin" validate-first :show-error="false" :show-error-message="false" @failed='onFailed'>
    <van-field
        v-model="user.mobile"
        icon-prefix="toutiao"
        left-icon="shouji"
        placeholder="请输入手机号"
        name="mobile"
        center
        :rules="fromRules.mobile"
    />
    <van-field
        v-model="user.code"
        clearable
        icon-prefix="toutiao"
        left-icon="yanzhengma"
        placeholder="请输入验证码"
        center
        name="code"
        :rules="fromRules.code"
    >
    <template #button>
      <van-count-down v-if="isCountDownShow" :time="1000 * 60" format="ss s" @finish="isCountDownShow = false" />
     <van-button @click.prevent="onSendSms" :loading="isSendSmsLoading" class="send-btn" size="small" round>发送验证码</van-button>
    </template>
    </van-field>
      <div class="login-btn-wrap">
        <van-button class="login-btn" type="info" block>登录</van-button>
      </div>
    </van-form>
  </div>
</template>

<script>
import { login, sendSms } from '@/api/user'
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
      },
      isCountDownShow: false,
      isSendSmsLoading: false
    }
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {
    async onLogin () {
      this.$toast.loading({
        message: '登录中...',
        forbidClick: true,
        duration: 0
      })
      try {
        const { data } = await login(this.user)
        this.$toast.success('登录成功')
        this.$store.commit('setUser', data.data)
        this.$router.back()
      } catch (err) {
        console.log(err)
        this.$toast.fail('登录失败，手机号或验证码错误')
      }
    },
    onFailed (error) {
      if (error.errors[0]) {
        this.$toast({
          message: error.errors[0].message,
          position: 'top'
        })
      }
    },
    async onSendSms () {
      try {
        const validateRet = await this.$refs['login-form'].validate('mobile')
        console.log(validateRet)
        this.isSendSmsLoading = true
        const res = await sendSms(this.user.mobile)
        console.log(res)
        this.isCountDownShow = true
      } catch (err) {
        let message = ''
        if (err && err.response && err.response.status === 429) {
          message = '发送频繁，请稍后'
        } else if (err.name === 'mobile') {
          message = err.message
        } else {
          message = '发送失败，请重试'
        }
        this.$toast({
          message,
          position: 'top'
        })
      }
      this.isSendSmsLoading = false
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
