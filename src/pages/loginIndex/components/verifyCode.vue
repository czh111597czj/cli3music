<template>
  <div class="code">
    <van-password-input :value="value"
                        :length="4"
                        :gutter="15"
                        :focused="showKeyboard"
                        @focus="showKeyboard = true" />
    <van-number-keyboard :show="showKeyboard"
                         @input="onInput"
                         @delete="onDelete"
                         @blur="showKeyboard = false" />
  </div>
</template>

<script>
import api from 'api'
import { getPhone } from 'utils/getPhone'

export default {
  name: 'verifyCode',
  data () {
    return {
      value: '',
      showKeyboard: true
    }
  },
  methods: {
    async onInput (key) {
      this.value = (this.value + key).slice(0, 4)
      if (this.value.length === 4) {
        const nickname = localStorage.getItem('nickname')
        const phone = getPhone()
        const password = this.password
        const captcha = this.value
        const { data } = await api.registerFn(captcha, phone, password, nickname)
        console.log(data)
        console.log('发送请求')
      }
    },
    onDelete () {
      this.value = this.value.slice(0, this.value.length - 1)
    }
  }

}
</script>

<style lang="less" scoped>
.code /deep/ li {
  border-bottom: 2px solid #aaa;
}
</style>
