<template>
  <div class="login-warp">
    <el-form class="login-form" label-position="top" label-width="80px" :model="formdata">
      <h2>用户登录</h2>
      <el-form-item label="用户名">
        <el-input v-model="formdata.username"></el-input>
      </el-form-item>
      <el-form-item label="密码">
        <el-input v-model="formdata.password"></el-input>
      </el-form-item>
      <el-button
      @click="hanleLogin()"
      class="login-btn" type="success">登录</el-button>
    </el-form>
  </div>
</template>

<script>
export default {
  data () {
    return {
      formdata: {
        username: '',
        password: ''
      }
    }
  },
  methods: {
    // 发送登录
    async hanleLogin () {
      const res = await this.$http.post(`login`, this.formdata)
      console.log(res)
      const {data: {data: {token}, meta: { msg, status }}} = res
      if (status === 200) {
        // console.log('success-----')
        // 使用localstorage 本地存储保存token
        localStorage.setItem('token', token)
        // 成功跳转至home组件-> js方式改标识
        this.$router.push({
          name: 'home'
        })
        this.$message.success('登录成功')
      } else {
        this.$message.error(msg)
      }
      /**
      this.$http.post(`login`, this.formdata)
      .then((res) => {
        console.log(res)
        // 对象解构赋值
        const {
          data: {
            data,
            meta: { msg, status }}} = res
        if (status === 200) {
          // console.log('success-----')
          // 成功跳转至home组件-> js方式改标识
          this.$router.push({
            name: 'home'
          })
        } else {
          this.$message.error(msg)
        }
      })
        .catch((err) => {
          console.log(err)
        })
       */
    }
  }
}
</script>

<style>
.login-warp {
  height: 100%;
  background-color: #324152;
  display: flex;
  justify-content: center;
  align-items: center;
}
.login-form {
  background-color: #ffffff;
  border-radius: 5px;
  width: 400px;
  padding: 30px;
}
.login-btn {
  width: 100%;
}
</style>
