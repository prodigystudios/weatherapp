<template>
    <video autoplay loop muted>
        <source :src="background">
    </video>
</template>

<script>
export default {
    name: "AnimatedBackground",
    props: ['animatedBgToUse'],
    data() {
        return {
            backgrounds: [
                {
                    bg: require('@/assets/AnimatedBackground/Test.mp4')
                },
                {
                    bg: require('@/assets/AnimatedBackground/RooftopClouds.mp4')
                }
            ],
            background: require('@/assets/AnimatedBackground/Test.mp4'),
        }
    },

    created() {
        if (this.animatedBgToUse == 'Partially cloudy' ||
            this.animatedBgToUse == 'Overcast' ||
            this.animatedBgToUse == 'Rain') {
            this.background = this.backgrounds[1].bg
        }
        else if (this.animatedBgToUse == 'clear-day') {
            this.background = this.backgrounds[0].bg;
        }
    }
}
</script>
<style scoped>
source
{
    width: 1920px;
    object-fit: fill;
}

.fade-enter-active,
.fade-leave-active
{
    transition: opacity 1.5s ease;
}

.fade-enter-from,
.fade-leave-to
{
    opacity: 0;
}

@media (max-width: 425px)
{

    source
    {
        width: 100%;
    }

    video
    {
        width: 100%;
        height: 3500px;
        object-fit: cover;
    }
}

@media (max-width: 650px)
{

    source
    {
        width: 100%;
    }

    video
    {
        width: 100%;
        height: 3000px;
        object-fit: cover;
    }
}
</style>