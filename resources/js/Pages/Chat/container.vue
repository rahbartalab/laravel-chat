<script>
import AppLayout from '@/Layouts/AppLayout.vue';
import MessageContainer from "@/Pages/Chat/MessageContainer.vue";
import InputMessage from "@/Pages/Chat/inputMessage.vue";
import ChatRoomSelection from "@/Pages/Chat/chatRoomSelection.vue";

export default {
    components: {
        ChatRoomSelection,
        AppLayout,
        MessageContainer,
        InputMessage
    },
    data: function () {
        return {
            chatRooms: [],
            currentRoom: [],
            messages: []
        }
    },
    methods: {
        getRooms() {
            axios.get('/chat/rooms')
                .then(response => {
                    this.chatRooms = response.data
                    this.setRoom(response.data[0])
                })
                .catch(error => {
                    console.log(error)
                })
        },
        setRoom(room) {
            this.currentRoom = room
            this.getMessages()
        },
        getMessages() {
            axios.get('/chat/room/' + this.currentRoom.id + '/messages')
                .then(response => {
                    this.messages = response.data
                })
                .catch(error => {
                    console.log(error)
                })
        }
    },
    created() {
        this.getRooms()
    }
}
</script>

<template>
    <AppLayout title="Dashboard">
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                <ChatRoomSelection
                    v-if="currentRoom.id"
                    :chatRooms="chatRooms"
                    :currentRoom="currentRoom"
                    v-on:roomChanged="setRoom($event)"
                />
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-xl sm:rounded-lg">
                    <MessageContainer
                        :messages="messages"
                    />
                    <InputMessage
                        :room="currentRoom"
                        v-on:messageSent="getMessages"
                    />
                </div>
            </div>
        </div>
    </AppLayout>
</template>
