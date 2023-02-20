<template>
    <div class="wrapper">
        <div class="right_body">
        <div class="allMessages">
            <div class="messages" 
            v-for="message in chat.history"
            :key="message.id"
            >
            <div class="from">
                <h3>{{ message.from }}</h3>
                <p>{{ message.text }}</p>
                <p>15:16</p>
            </div>
            <div class="me">
                <h3>{{login}}</h3>
                <p>It is a long established fact that a reader will be distracted by the readable content of a page when looking at its layout. The point of using Lorem Ipsum is that it has a more-or-less normal distribution of letters, as opposed to using 'Content here, content here', making it look like readable English. Many desktop publishing packages and web page editors now use Lorem Ipsum as their default model text, and a search for 'lorem ipsum' will uncover many web sites still in their infancy. Various versions have evolved over the years, sometimes by accident, sometimes on purpose (injected humour and the like).</p>
                <p style="text-align: right;">15:16</p>
            </div>
        </div>

        </div>

        <div class="input">
            <input>
            <button>Отправить</button>
        </div>
    </div>
    </div>
</template>

<script>
    export default{
        name: 'MessageList',
        props:{
            chat:{
                type: Object,
                default: {}
            },
            login:{
                type: String,
                default: ''
            }
        },

    }
</script>

<style scoped lang="less">

.right_body{
    position: fixed;
    z-index: 0;
    .allMessages{
        width: 900px;
        margin-top: 10px;
        overflow: auto;
        scrollbar-color: red;
        height: 500px;
    }
        .messages{
            display: flex;
            background-color: rgba(128, 128, 128, 0.17);
            flex-direction: column;
            padding: 10px;
            gap: 20px;
            .from{
                border-radius: 30px;
                max-width: 450px;
                background-color: rgba(0, 0, 255, 0.696);
                padding: 10px 10px 0 10px;
            }
            .me{
                border-radius: 30px;
                align-self: flex-end;
                max-width: 450px;
                background-color: rgba(255, 0, 0, 0.604);
                padding: 10px 10px 0 10px;
                h3{
                    text-align: right;
                }

                
            }
        }
        input{
            margin: 0 0 0 60px;
            width: 750px;
            height: 20px;
        }
    }

</style>