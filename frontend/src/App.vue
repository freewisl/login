<template>
  <div class="app">
    <div v-if="state.account.id">
      <p>안녕하세요? {{ state.account.name }}님!</p>
      <button @click="logout()">로그아웃</button>
    </div>
    <div v-else>
      <label for="loginId">
        <span>아이디</span>
        <input type="text" id="loginId" v-model="state.form.loginId" />
      </label>
      <label for="loginPw">
        <span>패스워드</span>
        <input type="password" id="loginPw" v-model="state.form.loginPw" />
      </label>
      <hr />
      <button @click="submit()">로그인</button>
    </div>
  </div>
</template>
<script>
import axios from 'axios'
import { reactive } from 'vue'
export default {
  components: {},
  data() {
    return {
      sampleData: ''
    }
  },
  setup() {
    const state = reactive({
      account: {
        id: null,
        name: ''
      },
      form: {
        loginId: '',
        loginPw: ''
      },
      loggedIn: false
    })

    const submit = () => {
      const args = {
        loginId: state.form.loginId,
        loginPw: state.form.loginPw
      }
      axios
        .post('api/account', args)
        .then((res) => {
          alert('로그인에 성공했습니다.')
          state.account = res.data
        })
        .catch(() => {
          alert('로그인에 실패했습니다.')
        })
    }

    const logout = () => {
      axios.delete('/api/account').then(() => {
        alert('로그아웃 하였습니다.')
        state.account.id = null
        state.account.name = ''
      })
    }

    axios.get('/api/account').then((res) => {
      state.account = res.data
    })

    return { state, submit, logout }
  },
  created() {},
  mountec() {},
  unmounted() {},
  methods: {}
}
</script>
