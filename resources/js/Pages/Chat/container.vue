<template>
    <AppLayout title="Dashboard">
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                <ChatRoomSelection
                    V-if="currentChatRoom.id"
                    :rooms="chatRooms"
                    :currentRoom="currentChatRoom"
                    v-on:roomselected="setRoom($event)"
                />
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div class="bg-white overflow-hidden shadow-xl sm:rounded-lg">
                    <MessageContainer :messages="messages" />
                    <inputMessage
                        :room="currentChatRoom"
                        v-on:messagesent="getMessages()"
                    />
                </div>
            </div>
        </div>
    </AppLayout>
</template>

<script>
import AppLayout from "@/Layouts/AppLayout.vue";
import MessageContainer from "./messageContainer.vue";
import InputMessage from "./inputMessage.vue";
import ChatRoomSelection from "./chatRoomSelection.vue";
export default {
    components: {
        AppLayout,
        MessageContainer,
        InputMessage,
        ChatRoomSelection,
    },
    data: function () {
        return {
            chatRooms: [],
            currentChatRoom: [],
            messages: [],
        };
    },
    watch: {
        currentChatRoom(val, oldVal) {
            if (oldVal.id) this.disconnect(oldVal);
            this.conenct();
        },
    },
    methods: {
        conenct() {
            if (this.currentChatRoom.id) {
                let vm = this;
                this.getMessages();
                window.Echo.private("chat." + this.currentChatRoom.id).listen(
                    ".message.new",
                    (e) => {
                        vm.getMessages();
                    }
                );
            }
        },
        disconnect(room) {
            window.Echo.leave("chat." + room.id);
        },
        getRooms() {
            axios
                .get("/chat/rooms")
                .then((response) => {
                    this.chatRooms = response.data;
                    this.setRoom(this.chatRooms[0]);
                })
                .catch((error) => {
                    console.log(error);
                });
        },
        setRoom(room) {
            this.currentChatRoom = room;
        },
        getMessages() {
            axios
                .get("/chat/room/" + this.currentChatRoom.id + "/messages")
                .then((response) => {
                    this.messages = response.data;
                })
                .catch((error) => {
                    console.log(error);
                });
        },
    },
    created() {
        this.getRooms();
    },
};
</script>
