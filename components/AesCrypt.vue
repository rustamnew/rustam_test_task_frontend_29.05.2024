<script setup>
import CryptoJS from 'crypto-js'

const cryptKey = ref('')
const textToCrypt = ref('')
const cryptedText = ref('')

const decryptKey = ref('')
const textToDecrypt = ref('')
const decryptedText = ref('')

const ivKey = 'a2xhcgAAAAAAAAAA'

function encryptData() {
    if (textToCrypt.value) {
        const key = CryptoJS.PBKDF2(cryptKey.value, 'salt', { keySize: 256 / 32, iterations: 100 })
        const iv = CryptoJS.enc.Utf8.parse(ivKey) // Convert string to WordArray
        const encrypted = CryptoJS.AES.encrypt(textToCrypt.value, key, { iv, mode: CryptoJS.mode.CBC })
        cryptedText.value = encrypted.ciphertext.toString(CryptoJS.enc.Hex)
    }
}

function decryptData() {
    if (textToDecrypt.value) {
        const key = CryptoJS.PBKDF2(decryptKey.value, 'salt', { keySize: 256 / 32, iterations: 100 })
        const iv = CryptoJS.enc.Utf8.parse(ivKey)
        const decrypted = CryptoJS.AES.decrypt({ ciphertext: CryptoJS.enc.Hex.parse(textToDecrypt.value) }, key, { iv, mode: CryptoJS.mode.CBC })
        decryptedText.value = decrypted.toString(CryptoJS.enc.Utf8)
    }
}
</script>

<template>
    <div class="w-full max-w-96">
        <div class="mt-6 mb-12">
            <h3 class="text-h3 mb-4">
                Зашифровать в AES
            </h3>
            <q-input v-model="cryptKey" placeholder="Ключ" class="mb-2" />
            <q-input v-model="textToCrypt" type="textarea" placeholder="Текст" class="w-full mb-6" />

            <div v-if="cryptedText" class="mb-4">
                <h4 class="text-lg mb-2">
                    Зашифрованный текст:
                </h4>
                <p class="text-lg break-all">
                    {{ cryptedText }}
                </p>
            </div>

            <q-btn label="Зашифровать" @click="encryptData()" />
        </div>

        <div class="mb-6">
            <h3 class="text-h3 mb-4">
                Расшифровать AES
            </h3>
            <q-input v-model="decryptKey" placeholder="Ключ" class="mb-2" />
            <q-input v-model="textToDecrypt" type="textarea" placeholder="Текст" class="w-full mb-6" />

            <div v-if="decryptedText" class="mb-4">
                <h4 class="text-lg mb-2">
                    Расшифрованный текст:
                </h4>
                <p class="text-lg break-all">
                    {{ decryptedText }}
                </p>
            </div>

            <q-btn label="Расшифровать" @click="decryptData()" />
        </div>
    </div>
</template>
