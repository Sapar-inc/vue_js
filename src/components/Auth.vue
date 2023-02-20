<template>
<div class="body">
    <div id="example-1">
    </div>
    <p>Логин</p>
    <input v-model="auth_data.login"/>
    <p>Пароль</p>
    <input v-model="auth_data.password"/><br>
    <button v-on:click="authorization_click_handler">Вход</button>
</div>
    
</template>

<script>
    export default{
        name: "Auth",    
        data: () => ({
    server_url: 'http://195.49.210.34/',
    auth_data: {
      login: '',
      password: '',
    //   tryPassword: ''
    },
    // response: ''
  }),
  methods: { 
    authorization_click_handler: async function () {
      if(!this.auth_data.login || !this.auth_data.password) return alert('Error');

      // console.log({response: this.response});

      const request_body = {
        userData: {
          login: this.auth_data.login,
          password: this.auth_data.password
        }
      };

      const response = await this.send_request('user/authorization', 'POST', request_body);
      console.log({response});

      if(response.info.status == "OK" && response.payload.isAuth) {
        this.$router.push({name: 'ChatsView', params: {id: response.payload.userData[0]._id}})
      } 
      else {
        alert('Incorrect login password')
      }
      console.log({response});
    },
    send_request: async function (path, method, body) {
      const request_config = {
        method,
        headers: {
          'Content-type': 'application/json;charset=utf-8'
        },
        body: null
      }

      if(method !== 'GET') request_config.body = JSON.stringify(body);
      console.log(JSON.stringify(body));

      // fetch(${this.server_url}${path}, request_config)
      // .then(response => {
      //     response.json()
      //     .then(jsonRes => {
      //         this.response = jsonRes;
      //     })
      //     .catch(error => {
      //         console.log({error});
      //     })
      // })
      // .catch(error => {
      //     console.log({error});
      // })

      let data = await fetch(`${this.server_url}${path}`, request_config)

      return await data.json();

      // console.log({response});
    }
  },

    }




</script>

<style scoped lang="less">
 .body{
    margin: auto;
    background-color: grey;
    width: 500px;
    height: 400px;
    border-radius: 30px;
    text-align: center; 
    padding-top: 100px;
    
 }
 p{
    margin: 0;
    font-size: 30px;
    font-family:'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
 }
 input{
    width: 250px;
    height: 25px;
    text-align: center;
    font-size: 20px;
    color: rgb(26, 174, 0);
    border-radius: 10px;
    border: 0;
    font-family:'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
    &:focus{
        background-color: rgb(177, 177, 177);
        border: 0;
        outline: none;
    }
 }
 button{
    width: 150px;
    height: 30px;
    font-size: 20px;
    margin: 15px 10px 0 10px;
    background-color: orange;
    border: 0;
    font-family:'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
    &:active{
        color: rgb(54, 54, 54);
        background-color: rgb(160, 104, 0);
    }
 }
</style>

