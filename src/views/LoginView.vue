<template>
  <div class="home">
    <div class="switcher">
      <button class="auth" v-on:click="switcher_handker(false)">Авторизация</button>
      <button class="reg" v-on:click="switcher_handker(true)">Регистрация</button>
    </div>
<div class="form">
  <Auth v-if="!switcher"/>
  <Reg v-if="switcher"/>
</div>

  </div>
</template>

<script>
import Auth from '@/components/Auth.vue'
import Reg from '@/components/Reg.vue'

export default {
  name: 'login',
  components: {
    Auth,
    Reg
  },
  data: () =>({
    switcher: false
  }),
  methods: {
    switcher_handker: function(isSwitch){
      this.switcher = isSwitch
    }
  }
}
</script>

<style scoped lang="less">
  .home{
    text-align: center;
    
  }
  .switcher{
    & *{
      width: 250px;
      height: 25px;
      margin: 0 5px 10px 5px;
      background-color: orange;
      border: 0;
      font-family:'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
      font-size: 20px;
      border-radius: 30px;
      cursor: pointer;
      &:active{
        background-color: rgb(177, 118, 8);
      }
    }
  }
</style>
