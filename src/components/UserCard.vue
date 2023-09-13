<script setup>
defineProps(['user'])
const emit = defineEmits(['select-user'])
</script>


<template>
    <div class="user-card__container" @click="emit('select-user', user.login)">
       <img :src="user.avatar_url" class="user-card__image"/>
       <a :href="user.html_url" target="_blank">{{ user.login }}</a>
    </div>
</template>

<style scoped>
.user-card__container {
    min-height: 200px;
    border: 1px solid grey;
    width: 300px;
    padding: 16px;
}
.user-card__image {
    height: 32px;
    width: 32px;
    border-radius: 50%;
}
</style>