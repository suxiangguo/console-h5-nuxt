<template>
  <div class="layui-container">
    <ul class="layui-nav layui-bg-white" lay-filter>
      <li class="layui-nav-item layui-this">
        <router-link :to="{ name: 'login' }">登录</router-link>
      </li>
      <li class="layui-nav-item">
        <router-link :to="{ name: 'reg' }">注册</router-link>
      </li>
    </ul>
    <validation-observer ref="observer">
      <form class="layui-form layui-form-pane">
        <div class="layui-form-item">
          <label class="layui-form-label">邮箱</label>
          <validation-provider rules="required|email" v-slot="{ errors }">
            <div class="layui-input-inline">
              <input
                type="text"
                name="email"
                placeholder="请输入邮箱"
                autocomplete="off"
                class="layui-input"
                v-model.trim="email"
              />
            </div>
            <div class="layui-form-mid layui-word-aux">
              <span>{{ errors[0] }}</span>
            </div>
          </validation-provider>
        </div>
        <div class="layui-form-item">
          <label class="layui-form-label">密码</label>
          <validation-provider
            rules="required|max:16|min:6"
            v-slot="{ errors }"
          >
            <div class="layui-input-inline">
              <input
                type="password"
                name="password"
                v-model="password"
                placeholder="请输入密码"
                autocomplete="off"
                class="layui-input"
              />
            </div>
            <div class="layui-form-mid layui-word-aux">{{ errors[0] }}</div>
          </validation-provider>
        </div>
        <div class="layui-form-item captcha">
          <label class="layui-form-label">验证码</label>
          <validation-provider
            ref="codefield"
            rules="required|length:4"
            v-slot="{ errors }"
          >
            <div class="layui-input-inline">
              <input
                type="text"
                name="code"
                v-model="code"
                placeholder="请输入验证码"
                autocomplete="off"
                class="layui-input"
              />
            </div>
            <div class="layui-form-mid layui-word-aux">{{ errors[0] }}</div>
            <div
              class="layui-form-mid layui-word-aux svg"
              v-html="codeInfo.svg"
              @click="_getCode()"
            ></div>
          </validation-provider>
        </div>

        <div class="submit">
          <button class="layui-btn" type="button" @click="submit()">
            立即登录
          </button>
          <router-link :to="{ name: 'forget' }" class="btn-hover"
            >忘记密码</router-link
          >
        </div>
      </form>
    </validation-observer>
  </div>
</template>

<script>
import code from '~/mixins/code'

export default {
  name: 'Login',
  mixins: [code],
  data() {
    return {
      email: '',
      password: '',
    }
  },
  methods: {
    submit() {
      // const isValid = await this.$refs.observer.validate()
      // if (!isValid) {
      //   return false
      // }
      this.$auth
        .loginWith('local', {
          data: {
            email: this.email,
            password: this.password,
            code: this.code,
            sid: this.sid,
          },
        })
        .then((res) => {
          console.log(res)
          if (res.data.code === 10000) {
            alert('登陆成功')
          } else {
            alert(res.data.message)
          }
        })
      // login({
      //   email: this.email,
      //   password: this.password,
      //   code: this.code,
      //   sid: this.$store.state.sid,
      // }).then((res) => {
      //   if (res.code === 10000) {
      //     this.$pop('登录成功')
      //     this.$store.commit('setUserInfo', res.data.userJson)
      //     this.$store.commit('setUserToken', res.data.token)
      //     this.$store.commit('setLoginStatus', true)
      //     this.$router.push({ name: 'index' })
      //   } else if (res.code === 9003) {
      //     this.$refs.codefield.setErrors([res.message])
      //   } else {
      //     this.$alert(res.message)
      //   }
      // })
    },
  },
  mounted() {},
}
</script>
<style lang="scss" scoped>
.layui-container {
  background-color: #fff;
  overflow: hidden;
}

.layui-bg-white {
  background-color: #fff;
  color: #333;
  margin-bottom: 10px;
}

.layui-nav .layui-nav-item a {
  color: #333;
}

.layui-input-block,
.layui-input-inline {
  input {
    width: 250px;
  }
}

.layui-form-item .layui-input-inline {
  width: 250px;
}

.captcha {
  height: 80px;
  .svg {
    position: relative;
    padding: 0 !important;
    height: 60px;
    clear: both;
    margin-left: 110px;
  }
}

.btn-hover {
  margin-left: 20px;
  &:hover {
    color: #009688;
  }
}

.submit {
  margin: 40px 0 20px;
}
</style>
