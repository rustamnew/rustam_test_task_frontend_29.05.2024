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
    <TextToSha256 />
    <AesCrypt />
</template>
