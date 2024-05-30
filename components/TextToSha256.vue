<script setup>
import { sha256 } from 'js-sha256'

const textToHash = ref(null)
const hash = ref(null)

function hashString() {
    if (textToHash.value)
        hash.value = sha256(textToHash.value)
}
</script>

<template>
    <div class="w-full my-6 max-w-96">
        <q-input
            v-model="textToHash"
            filled
            type="textarea"
            placeholder="Введите текст"
            class="w-full mb-6"
        />

        <div v-if="hash" class="mb-4">
            <h4 class="text-lg mb-2">
                SHA256:
            </h4>
            <p class="text-lg">
                {{ hash }}
            </p>
        </div>

        <q-btn color="primary" label="Получить sha256" @click="hashString()" />
    </div>
</template>
