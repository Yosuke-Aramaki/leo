<template>
  <div>
    <div class="section-logo">
      <img src="/images/leo_icon_header.png" class="logo" />
    </div>
    <div class="box-frame">
      <p class="error">{{ this.errors }}</p>
      <div class="box-frame-title">
        <h2>Welcome back!</h2>
      </div>
      <form @submit.prevent="signin">
        <input class="form-field" type="text" v-model="form_login.email" placeholder="email" name="email" autocomplete=”on”/>
        <input class="form-field" type="password" v-model="form_login.password" placeholder="password" name="password" autocomplete=”on”/>
        <button class="form-button" type="submit">ログイン</button>
      </form>
      <nuxt-link to="/password_reset_request" no-prefetch>
        <div class="link">パスワードを忘れた方はこちら</div>
      </nuxt-link>
    </div>
  </div>
</template>

<script>
import Error from '@/components/error'

export default {
  head() {
    return {
      title: 'signin'
    }
  },
  components: {
    Error
  },
  data: () => ({
    form_login: {
      email: '',
      password: ''
    },
    errors: '',
  }),
  methods: {
    async signin () {
      this.$axios.$post('api/v1/signin', {session: this.form_login })
      .then((response) => {
        this.$store.dispatch('auth/set_user_token', response )
        location.replace('/')
      })
      .catch((error) => {
        if (error.response && error.response.status === 401) {
          this.errors = []
          this.errors = error.response.data.messages
        }
      })
    }
  }
}
</script>

<style scoped>
.section-logo {
  margin: 20px 0px;
  margin-top: 100px;
  text-align: center;
}

.logo {
  height: 70px;
}

.box-frame {
  width: 400px;
  margin: 0 auto;
  padding: 25px 40px;
  text-align: center;
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: rgba(0, 0, 0, 0.1) 0px 0px 10px 0px;
}

.box-frame-title h2 {
  margin-top: 4px;
  margin-bottom: 35px;
}

.form-field {
  width: 100%;
  font-size: 13px;
  height: 40px;
  padding-left: 8px;
  margin-bottom: 20px;
  background-color: #FAFBFC;
  border: 2px solid #DFE1E6;
  box-sizing: border-box;
  border-radius: 4px;
}

.form-button {
  width: 100%;
  height: 40px;
  font-size: 14px;
  font-weight: 600;
  box-sizing: border-box;
  display: inline-block;
  border-radius: 8px;
  border: 0;
  cursor: pointer;
  padding: auto 18px;
  background: #1E65DC;
  color: #ffffff;
  transition: .2s;
  /* box-shadow: 6px 6px 3px #666666;   */
  /* border: 2px solid #ffffff;  */
}

.form-button:hover {
  background: #7aa3eb;
}

.link {
  margin: 18px 16px;
  font-size: 14px;
}

.error {
  font-size: 12px;
  color: #FF954A;
}
</style>
