<template>
    <button @click="onClick" v-bind:style="{background:color}" class="btn">{{ text }}</button>
</template>

<script>
    export default {
        name: 'TheButton',
        props: {
            text: String,
            color: String
        },
        methods: {
            onClick() {
                this.$emit('btn-click')
            }
        }
    }
</script>