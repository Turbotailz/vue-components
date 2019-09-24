<template>
    <a :href="href">{{ label }}</a>
</template>

<script>
    export default {
        name: 'button-link',

        props: {
            label: {
                required: true,
                type: String,
            },
            href: {
                required: true,
                type: String,
            }
        }
    }
</script>

<style lang="scss" scoped>
    a {
        background: dodgerblue;
        color: white;
        display: inline-block;
        padding: 1rem 2rem;
        border: 2px solid dodgerblue;

        &:hover {
            background: white;
            color: dodgerblue;
        }
    }
</style>