<template>
    <figure v-if="slides.length" class="image-slider">
        <transition :name="imageTransition" mode="in-out">
            <div class="image-container" :key="'slide' + currentSlideIndex">
                <img ref="sliderImage" :src="currentSlide.image" :alt="currentSlide.caption">
            </div>
        </transition>

        <a v-if="currentSlide.link" :href="currentSlide.link" class="mask"></a>
        <div v-else class="mask"></div>

        <figcaption>
            <transition :name="captionTransition" mode="out-in">
                <span :key="currentSlideIndex">
                    {{ currentSlide.caption }}
                </span>
            </transition>

            <nav v-if="slides.length > 1">
                <ul>
                    <li v-for="(slide, index) in slides"
                        :class="{ current: slide === currentSlide }"
                        @click="goToSlide(index)"
                    >
                        <span v-if="slide === currentSlide || currentSlideIndex > index"
                              :style="{ animationDuration: interval / 1000 + 's' }"
                        ></span>
                    </li>
                </ul>
            </nav>
        </figcaption>
    </figure>
</template>

<script>
    export default {
        name: 'image-slider',
        data() {
            return {
                currentSlideIndex: 0,
                timer: setInterval(() => {
                    this.goToNextSlide();
                }, this.interval),
            }
        },
        props: {
            slides: Array,
            interval: {
                type: Number,
                default: 5000,
            },
            imageTransition: {
                type: String,
                default: 'fade'
            },
            captionTransition: {
                type: String,
                default: 'fade'
            }
        },
        computed: {
            currentSlide() {
                if (this.slides.length) return this.slides[this.currentSlideIndex];
                return null;
            },
            slideLength() {
                return this.slides.length - 1;
            }
        },
        methods: {
            goToSlide(index) {
                clearInterval(this.timer);

                this.timer = setInterval(() => {
                    this.goToNextSlide();
                }, this.interval);

                this.currentSlideIndex = index;
            },
            goToNextSlide() {
                if (this.currentSlideIndex === this.slideLength) {
                    this.currentSlideIndex = 0;
                } else {
                    this.currentSlideIndex++;
                }
            },
        },
        mounted() {
            // Pre-load images
            this.slides.forEach(slide => {
                let image = new Image();
                image.src = slide.image;
            });
        }
    }
</script>

<style lang="scss">
    .image-slider {
        flex: 1 0 auto;
        position: relative;
        min-height: 400px;

        .image-container {
            position: absolute;
            top: 0;
            bottom: 0;
            left: 0;
            right: 0;
            z-index: 1;
            overflow: hidden;
            height: 100%;

            img {
                position: absolute;
                top: 0;
                left: 0;
                width: 100%;
                height: 100%;
                object-fit: cover;
                object-position: center;
            }
        }

        .mask {
            position: absolute;
            top: 0;
            bottom: 0;
            left: 0;
            right: 0;
            z-index: 2;
            background: linear-gradient(to bottom, transparent, rgba(0, 0, 0, .5));
        }

        figcaption {
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            z-index: 3;
            color: #FFF;
            padding: 0 4rem 4rem;
            display: flex;
            flex-direction: column;

            > span {
                font-size: 1.25rem;
                font-weight: 600;
                max-width: 66%;

                a {
                    color: #FFF;
                }
            }

            nav {
                margin-top: 1.5rem;

                ul {
                    display: flex;
                    list-style: none;
                    margin: 0;
                    padding: 0;

                    li {
                        background: rgba(255, 255, 255, .25);
                        display: block;
                        height: .5rem;
                        width: 8rem;
                        margin-right: 1rem;
                        cursor: pointer;
                        position: relative;

                        span {
                            position: absolute;
                            left: 0;
                            right: 0;
                            height: 100%;
                            background: #FFF;
                        }

                        &.current {
                            background: rgba(255, 255, 255, .5);

                            span {
                                right: 100%;
                                animation: slide-active linear;
                            }
                        }
                    }
                }
            }
        }
    }

    @keyframes slide-active {
        100% {
            right: 0;
        }
    }

    // fade
    .fade {
        &-enter-active, &-leave-active, &-move {
            transition: all .3s;
        }

        &-enter, &-leave-to {
            opacity: 0;
        }
    }
</style>