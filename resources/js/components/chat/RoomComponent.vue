<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-header">Chat Room</div>

                    <div id="chat-box" class="card-body bg-secondary">
                        <div class="median bg-light rounded">
                            <div class="media-body m-2">
                                <h5 class="mt-0">Username</h5>
                                Lorem ipsum dolor sit amet consectetur adipisicing elit. Consequuntur esse animi mollitia ab ducimus possimus placeat aspernatur quaerat, iure debitis perspiciatis. Vel accusamus vitae nulla ipsam possimus perspiciatis fuga autem.
                            </div>
                        </div>
                    </div>

                    <div class="card-footer">
                        <div class="form-group">
                            <input type="text" class="form-control" v-model="message" @keyup.enter="sendMessage()">
                        </div>
                        <p class="text-muted">Username typing...</p>
                    </div>

                </div>
            </div>

            <div class="col-md-4">
                <div class="card">
                    <div class="card-header">Users Online</div>
                    <div id="user-online" class="card-body bg-secondary">
                        <ul class="list-group">
                            <li class="list-group-item">Username</li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data(){
            return {
                message: '',
                messages: []
            }
        },

        props: {
            user: Object
        },

        methods: {
            sendMessage() {
                this.messages.push({
                    user: this.user,
                    message: this.message
                })

                axios.post('/send', {
                    message: this.message
                })

                .then((result) => {
                    console.log(result);
                }).catch((err) => {
                    console.log(err)
                });

                this.message = '';

            }
        },

        mounted() {
            Echo.join('chat')
                .listen('ChatSent', (e) => {
                    console.log(e);
                });
        }
    }
</script>
