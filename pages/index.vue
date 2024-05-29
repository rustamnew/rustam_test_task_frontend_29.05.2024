<script setup>
useHead({
    title: 'Главная',
})
definePageMeta({
    layout: 'default',
    name: 'Главная',
})
</script>

<template>
    <MetamaskWallet />
    <FrameComponent />
    <ToSha256 />
</template>
