<template>
        <div class="body">
        <p>Логин</p>
        <input v-model="reg_data.login" type="text" placeholder="Login" class="login">
        <p>Пароль</p>
        <input v-model="reg_data.password" type="text" placeholder="Password" class="password">
        <p>Пароль повторите еще раз</p>
        <input v-model="reg_data.tryPassword" type="text" placeholder="try password" class="tryPassword"><br>
        {{ response }}
        <button class="approve" v-on:click="registration_click_handler">Зарегистрироваться</button>
    </div>
    
</template>

<script>
    export default{
        name: "Reg",
        data: () => ({
    server_url: 'http://195.49.210.34/',
    reg_data: {
        login: '',
        password: '',
        tryPassword: ''
    },
    // response: ''
  }),
  methods: { 
    registration_click_handler: async function () {
        if(!this.reg_data.login || !this.reg_data.password || !this.reg_data.tryPassword) return alert('Error');

        // console.log({response: this.response});

        const request_body = {
            userData: {
                login: this.reg_data.login,
                password: this.reg_data.password,
                tryPassword: this.reg_data.tryPassword
            }
        };

        const response = await this.send_request('user/registration', 'POST', request_body);
        console.log({response});

        if(response.info.status == "Error") return alert(response.payload);
        else this.$router.push({name: 'home', params: {id: response.payload._id}})
        
        
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

        // fetch(`${this.server_url}${path}`, request_config)
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
  }
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
    width: 200px;
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
