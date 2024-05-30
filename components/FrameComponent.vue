<script setup>
import iframeApps from '../json/iframeApps.json'

const frameOpened = ref(false)
const frameLoaded = ref(false)
const frameURL = ref(false)

function openFrame(url) {
    frameOpened.value = true
    frameURL.value = url
}
function closeFrame() {
    frameOpened.value = false
    frameLoaded.value = false
    frameURL.value = null
}
</script>

<template>
    <div class="my-6">
        <h3 class="text-h3 mb-6">
            Приложения Iframe
        </h3>

        <div class="flex mb-3">
            <q-btn v-for="item, index in iframeApps" :key="index" :label="item.title" color="primary" class="m-1" @click="frameLoaded = false; openFrame(item.url)" />
        </div>

        <ClientOnly>
            <q-card v-if="frameOpened === true" class="relative m-6 mt-3">
                <div class="flex justify-end border-b-2 border-gray">
                    <q-btn icon="close" color="primary" @click="closeFrame()" />
                </div>

                <div v-if="frameLoaded === false" class="absolute top-1/3 w-full flex justify-center">
                    <q-spinner
                        color="primary"
                        size="3em"
                        class=""
                    />
                </div>

                <iframe :src="frameURL" width="600" height="600" @load="frameLoaded = true">
                    Ваш браузер не поддерживает плавающие фреймы!
                </iframe>
            </q-card>
        </ClientOnly>
    </div>
</template>
