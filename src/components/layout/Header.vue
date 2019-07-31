<template lang="pug">
    section.hero.is-success
        .hero-head
            header.navbar
                .container
                    .navbar-start
                        .navbar-item
                            strong 📻 platzi Music
                    .navbar-end.navbar-menu
                        router-link.navbar-item(to="/") Buscar
                        router-link.navbar-item(to="about") Nosotros

        .hero-body
            .container.has-text-centered
                h1.title Platzi Music
                h2.subtitle Canciones que estan Vue-nisimas
                pm-player


</template>

<script>
    import PmPlayer from '@/components/Player.vue'

    export default {
        name: "PmHeader",
        components: {
            PmPlayer
        }
    }
</script>

