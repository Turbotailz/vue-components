<template>
    <span>{{ animatedValue }}</span>
</template>

<script>
    import TweenLite from 'gsap';

    export default {
        name: 'animated-integer',
        props: {
            value: {
                type: Number,
                required: true
            },
            toFixed: {
                type: Number,
                default: 0,
            }
        },
        data() {
            return {
                tweeningValue: this.value,
            }
        },
        watch: {
            value: function (newValue) {
                TweenLite.to(this.$data, 0.5, { tweeningValue: newValue });
            }
        },
        computed: {
            animatedValue() {
                return this.tweeningValue.toFixed(this.toFixed);
            }
        }
    }
</script>