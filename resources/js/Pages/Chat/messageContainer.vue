<template>
    <div>
        <div class="flex flex-col h-full">
            <div class="flex-1 overflow-y-auto">
                <div class="flex flex-col-reverse">
                    <div v-for="message in messages" :key="message.id">
                        <MessageItem :message="message" />
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import MessageItem from "./messageItem.vue";

export default {
    components: {
        MessageItem,
    },
    props: ["messages"],
};
</script>
