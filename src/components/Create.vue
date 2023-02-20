<template>
  <div class="wrapper">
    <img src="@/assets/img_267727.png" v-on:click="createChatButtonHandler" class="close">
    <input v-model="label" placeholder="Название..." type="text">
    <input v-model="description" placeholder="Описание..." type="text">
    <hr>
    <h3>Выберите участников чата.</h3>
    <div class="addMemberWrapper">
      <input list="usersList" v-model="selectMember" placeholder="Выберите участника..." type="text">
      <div v-on:click="add_member_button_handler" class="addMemberButton button">Добавить</div>
    </div>
    <div class="addedMembersList">
      Добавленные участники: {{ selectedMembers }}
    </div>
    <div v-on:click="create_chat_button_handler" class="submit button">Создать</div>
    <datalist id="usersList">
      <option v-for="user in userList" 
              :key="user.id" 
              :value="user.login"
      />
    </datalist>
  </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'createChat',
    // в data мы объявляем все переменные
    data: () => ({
        label: '',
        description: '',
        selectMember: '',
        selectedMembers: [],
        serverUrl: 'http://195.49.210.34/',
        userList: [],
        isActive: false,
    }),
    async mounted (){
      this.userList = (await this.sendRequest('user', 'GET')).payload
    },
    methods: {
      createChatButtonHandler: function (){
            this.$emit('CloseModalCreateChat')
            }, 
        create_chat_button_handler: async function () {
        // проверка
        if(!this.label || !this.description) return alert('Вы не заполнили поля "Название" или "Описание"')
        
        // собираем все данные для создания чата
        const request_body = {
            userId: this.$route.params.id || "63e253d392021356e711e486", // id пользователя который хочет создать новый чат
            label: this.label, // название чата
            members: this.selectedMembers, // массив участников чата
            description: this.description // описание чата, необязательный параметр!
        }

        // отправляем запрос на создание чата
        const response = await this.sendRequest('chat/create', "POST", request_body);
        
        // проверяем удалось ли создать чат
        console.log({response});
    },
    
    add_member_button_handler: async function () {
        // делаем проверку для пользователя
        if (!this.selectMember) return alert('Вы не выбрали участника');
        if (this.selectedMembers.includes(this.selectMember)) return alert('Этого участника вы уже выбрали');
        let selectedMembersIsExit = false
        let counter = 0
        for(const user of this.userList){
          if(user.login === this.selectMember) {
            selectedMembersIsExit = true 
          }
          if(this.selectedMembers.includes(user.login)) counter++
          // if(this.selectMember !== user) return alert(`Пользователя с логинов: ${this.selectMember} не существует`)
        }
          if(!selectedMembersIsExit) return alert(`Пользователя с логинов: ${this.selectMember} не существует`)
          if(counter !== this.selectedMembers.length) return alert(`Пользователя с логинов: ${this.selectMember} не существует`)
        // сюда отправляем массив this.selectedMembers, в котором будет последнее значение массива selectMember
        this.selectedMembers.push(this.selectMember);
        this.selectMember = ''
    },
    sendRequest: async function (path, method, body) {
        const url = `${this.serverUrl}${path}`;
        
        if (method === 'GET') {
            return (await axios.get(url)).data
        } else {
            return (await axios.post(url, body)).data
        }

        // сокращенный вариант того же самого кода
        // axios - это какой то объект, у которого есть методы
        // method = method.toLowerCase()
        // return await axios[method](url).data
    }
}
}
</script>

<style scoped lang="less">
  .wrapper {
    background-color: rgb(251, 0, 0);
    width: 50vw;
    height: auto;
    display: flex;
    flex-direction: column;
    justify-content: space-evenly;
    align-items: center;
    position: relative;
    padding-top: 50px;
    border: 1px solid rgba(37, 47, 37, 1);
    padding: 20px;
    border-radius: 10px;
    .close {
      width: 2.5%;
      position: absolute;
      right: 5px;
      top: 5px;
      cursor: pointer;
      margin: 0;
    }
    .addMemberWrapper {
      width: 50%;
      display: flex;
      justify-content: space-evenly;
      align-items: center;
      input {
        width: 60%;
      }
    }
    .button {
      padding: 4px 16px;
      border: 1px solid rgba(37, 47, 37, 1);
      border-radius: 10px;
      cursor: pointer;
    }
    input {
      width: 40%;
      padding: 8px 16px;
      outline: none;
      border: 1px solid rgba(37, 47, 37, 1);
      border-radius: 10px;
    }
    * {
      margin-top: 20px;
    }
    hr {
      width: 100%;
    }
    h3 {
      padding: 0;
      margin: 0;
    }
  }
</style>