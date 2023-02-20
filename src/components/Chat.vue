<template>
<div class="home">
    <div class="left_body">
        <div class="user">
            <img class="user_img" src="@/assets/logo.png">
            <p class="user_name">{{login}}</p>
        </div>
        <div class="search_data" style="margin-top: 10px;">
                <input>
                <button v-on:click="createChatButtonHandler">+</button>
            </div>
    <div class="user_chats">
        <div class="list_chats">

            
            <div class="chat" 
            v-for="chat in availableChats" 
            v-bind:key="chat._id"
            @click="$event => choose_chat_button_handler(chat._id)"
            >
                <div class="chat_image">
                    <img src="@/assets/logo.png">
                </div>
                <div class="chat_content">
                    <div class="title">
                        {{chat.label}}
                    </div>
                    <div class="last_message">
                        <div class="from">
                            {{ chat.history.at(-1).from }} : &nbsp;
                        </div>
                        <div class="text">
                            {{ chat.history.at(-1).text.slice(0, 17) }}...
                        </div>
                    </div>
                </div>
            </div>
        </div>

    </div>

                
    </div>
            

</div>


</template>

<script>
import axios from 'axios'
export default {
    name: 'Chat',
    props:{
            // chat:{
            //     type: Object,
            //     default: {}
            // },
            login:{
                type: String,
                default: ''
            }
        },
    components: {

},  
    data: () => ({
            serverUrl: 'http://195.49.210.34/',
            availableChats: []
        }), 
        async mounted () {
            const response = await this.sendRequest('chat', 'GET');

            this.availableChats = response.payload;

            console.log(this.availableChats);
        },
        methods: { 
            createChatButtonHandler: function (){
            this.$emit('openModalCreateChat')
            },           
            sendRequest: async function(path, method, body) {
                const url = `${this.serverUrl}${path}`;
                let response;

                method = method.toLowerCase();

                return (await axios[method](url, body)).data;
            },
            choose_chat_button_handler: function (chat) {
                this.$emit('choose_chat', chat)
                console.log(chat)
            }
        }
    
}
</script>

<style scoped lang="less">
    .home{
        display: flex;
    }
    .left_body{
        border-right: 1px solid;
        padding: 10px 0 0 10px;
        width: 400px;
        .user{
            padding: 10px 0 0 10px;
            display: flex;
            align-items: center;
            border-bottom: 1px solid;
            gap: 10px;
            img{
                width: 100px;
                height: 100px;
                border-radius: 100px;
                background-color: red;
            }
            p{
                user-select: none;
                font-size: 35px;
            }
            &:hover{
                background-color: rgba(128, 128, 128, 0.553);
            }
        }
        .search_data{
                input{
                    width: 350px;
                    height: 20px;
                }
                button{
                    cursor: pointer;
                    width: 29px;
                    height: 29px;
                }
            }
        .user_chats{
            border: 1px solid;
            height: 350px;
            overflow: auto;

            .list_chats{
                .chat{
                    z-index: 0;
                    cursor: pointer;
                    display: flex;
                    margin: 10px 0 10px 0;
                    border: 1px solid;
                    width: 380px;
                    font-family:Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
                    img{
                        background-color: red;
                        border-radius: 100px;
                        margin:10px 0 0 10px;
                        width: 50px;
                    }
                    .chat_content{
                        padding: 10px 0 0 10px;
                        user-select:none;
                    }
                    &:hover{
                        background-color: gray;
                    }
                }
            }
        }
    }

</style>