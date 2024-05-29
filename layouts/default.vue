<template>
    <div class="main relative p-10">
        <slot />
    </div>
</template>

<style>
    html,
    body,
    #__nuxt {
        height: 100%;
    }
    html {
        font-size: 16px;
    }
    body {
        color: #0F0F0F;
        overflow-x: hidden;
    }
    #__nuxt {
        display: flex;
        flex-direction: column;
    }
    #__nuxt > * {
        flex-shrink: 0;
    }
    .main {
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    p {
        /*white-space: break-spaces;*/
        white-space: pre-line;
    }
    .disabled-block {
        pointer-events: none;
        user-select: none;
        opacity: 0.5;
    }
    .q-notification__actions {
        flex-wrap: nowrap
    }
</style>
