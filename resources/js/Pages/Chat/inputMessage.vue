<template>
    <div class="relative h-10 m-1">
        <div style="border-top: 1px solid #e6e6e6" class="grid grid-cols-6">
            <input
                type="text"
                v-model="message"
                @keyup.enter="sendMessage()"
                placeholder="Type your message here..."
                class="col-span-5 border-2 border-gray-300 bg-white h-10 px-5 pr-16 rounded-lg text-sm focus:outline-none focus:border-blue-500"
            />
            <button
                @click="sendMessage()"
                class="col-span-1 bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
            >
                Send
            </button>
        </div>
    </div>
</template>

<script>
// import Input from "@/Jetstream/Input.vue";
export default {
    // components: { Input },
    props: ["room"],
    data: function () {
        return {
            message: "",
        };
    },
    methods: {
        sendMessage() {
            if (this.message == "") return;
            axios
                .post("/chat/room/" + this.room.id + "/message", {
                    message: this.message,
                })
                .then((response) => {
                    if (response.status == 201) {
                        this.message = "";
                        this.$emit("messagesent");
                    }
                })
                .catch((error) => {
                    console.log(error);
                });
        },
    },
};
</script>
