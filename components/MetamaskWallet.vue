<script setup lang="ts">
import convertEth from 'ethereum-unit-converter'

declare global {
    interface WindowEventMap {
        'eip6963:announceProvider': CustomEvent
    }
}

const providerButtons: any = ref([])
const providerButtonsElement: any = ref(null)
const address: any = ref(null)
const walletData: any = ref(null)
const infura_api_key = '348VI7C73FE2MC5KP79S864EIT5A9GG6H7' // https://app.infura.io/
const etherBalance = ref(null)
const sendTo = ref(null)

// Connect to the selected provider using eth_requestAccounts.
async function connectWithProvider(wallet: EIP6963AnnounceProviderEvent['detail']) {
    sendTo.value = null
    etherBalance.value = null
    walletData.value = wallet

    try {
        const response: any = await wallet.provider.request({ method: 'eth_requestAccounts' })
        address.value = response[0]

        return response
    }
    catch (error) {
        console.error('Failed to connect to provider:', error)
    }
}

async function sendTransaction() {
    try {
        const response: any = await walletData.value.provider.request({
            method: 'eth_sendTransaction',
            params: [
                {
                    from: address.value,
                    to: sendTo.value,
                },
            ],
        })
        /*
        .then((result: any) => {
            console.log(result)
        })
        .catch((error: any) => {
            console.log(error)
        })
        */

        return response
    }
    catch (error) {
        console.error('Failed to connect to provider:', error)
    }
}

// Display detected providers as connect buttons.
function getProviders() {
    window.addEventListener('eip6963:announceProvider', (event: EIP6963AnnounceProviderEvent) => {
        providerButtons.value.push({
            icon: event.detail.info.icon,
            name: event.detail.info.name,
            eventDetail: event.detail,
        })
    })

    // Notify event listeners and other parts of the dapp that a provider is requested.
    window.dispatchEvent(new Event('eip6963:requestProvider'))
}

async function getInfo() {
    const balanceWei = await fetch(`https://api.etherscan.io/api?module=account&action=balance&address=${address.value}&tag=latest&apikey=${infura_api_key}`)
        .then(response => response.json())
        .then((data) => {
            return data
        })
        .catch((error) => {
            console.error(error)
        })

    const weiTotal = BigInt(balanceWei.result)

    etherBalance.value = convertEth(weiTotal, 'wei', 'ether')
}

onMounted(() => {
    getProviders()
})
</script>

<template>
    <div class="my-6">
        <div v-if="providerButtons.length" class="mb-6">
            <h3 class="text-h3 text-center mb-2">
                Кошельки:
            </h3>

            <div id="providerButtonsElement" ref="providerButtonsElement">
                <q-btn v-for="button, index in providerButtons" :key="index" class="m-2" stack @click="connectWithProvider(button.eventDetail)">
                    <q-img :src="button.icon" class="h-12 w-12 mb-1" />
                    <span>{{ button.name }}</span>
                </q-btn>
            </div>
        </div>

        <div v-if="address" class="mb-6">
            <h3 class="text-h3 mb-2">
                Адрес кошелька:
            </h3>

            <p class="text-lg mb-4">
                {{ address }}
            </p>

            <q-btn label="Получить баланс" @click="getInfo()" />
        </div>

        <div v-if="etherBalance !== null" class="mb-6">
            <h3 class="text-h3 mb-2">
                Баланс:
            </h3>

            <p class="text-lg">
                {{ etherBalance }}
            </p>
        </div>

        <div v-if="address && etherBalance !== null">
            <h3 class="text-h3 mb-2">
                Отправить транзакцию
            </h3>

            <q-input v-model="sendTo" label="Адрес для отправки" class="mb-6" />

            <q-btn label="Отправить транзакцию" @click="sendTransaction()" />
        </div>
    </div>
</template>
