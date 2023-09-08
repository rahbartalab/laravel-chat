<script>
import AppLayout from '@/Layouts/AppLayout.vue';
import MessageContainer from "@/Pages/Chat/MessageContainer.vue";
import InputMessage from "@/Pages/Chat/inputMessage.vue";

export default {
    components: {
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
        },
        setRoom(room) {
            this.currentRoom = room
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
                Chat
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-xl sm:rounded-lg">
                    <MessageContainer/>
                    <InputMessage/>
                </div>
            </div>
        </div>
    </AppLayout>
</template>
