<template>
    <div class="home">

        <div  class="chats" style="z-index: 0; position: absolute" >
            <Chats @openModalCreateChat="openModalCreateChat" @choose_chat ="choose_chat" :login="nameUser"/>
        </div>
        
        <div class="MessageLis">
                <MessageList :chat="currentChat" :login="nameUser"  class="MessageList "/>
        </div>
        
        
            <div class="createChatModalWrapper" style="z-index: 1; position: absolute">
            <div class="bacground" v-on:click="closeModel"></div>
            <div class="createChat">
                <CreateChat @CloseModalCreateChat="CloseModalCreateChat"/>
            </div>

        </div>
        
        
    </div>
</template>

<script>
import Chats from '@/components/Chat.vue'
import CreateChat from '@/components/Create.vue'
import MessageList from '@/components/MessageList.vue'
import axios from 'axios'

export default {
    name: 'ChatsView',
    components: {
        Chats,
        CreateChat,
        MessageList
    },
    data: () =>({
        createChatIsOpen: false,
        serverUrl: 'http://195.49.210.34/',
        user: {},
        nameUser: '',
        currentChat:{
            history: [
                {
                    text:'sda',
                    from: 'Sapar',
                    timestamp: '2023-02-18T08:13:29.259Z',
                    id: 1,

                },
                {
                    text:'Hi dude',
                    from: 'LolChel',
                    timestamp: '2023-02-18T08:13:29.259Z',
                    id: 2,
                },
                {
                    text:'what are u doing',
                    from: 'Debich228',
                    timestamp: '2023-02-18T08:13:29.259Z',
                    id: 3,
                }, 
            ]
        },
        // currentLogin: `${this.nameUser}`
    }),
    methods:{
        openModalCreateChat: function (){
            this.createChatIsOpen = true
            document.querySelector('.createChatModalWrapper').style.display = 'block'
        },
        CloseModalCreateChat: function (){
            document.querySelector('.createChatModalWrapper').style.display = 'none'
        },
        closeModel: function (){
            document.querySelector('.createChatModalWrapper').style.display = 'none'
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
        },

        choose_chat: function () {

        }

    },
    async mounted (){
        this.user = (await this.sendRequest(`user/${this.$route.params.id}`, 'GET')).payload[0].login
        this.nameUser = this.user
        console.log(this.user)
        console.log(this.nameUser)
    }
}

</script>

<style scoped lang="less">
    .bacground{
        position: absolute;
        background-color: rgba(128, 128, 128, 0.779);
        width: 100%;
        height: 100%;
        cursor: pointer;
        
    }
    .createChat{
        padding: 150px 0 0 23%;
    }
    .createChatModalWrapper{
        width: 100%;
        height: 100%;
        display: none;
    }
    .MessageLis{
        position: relative;
        left: 30%;
    }
</style>