<template lang="pug">
    .container(v-if="track && track.id")
        pm-loader(v-show="isLoading")
        .columns
            .column.is-3.has-text-centered
                figure.media-left
                    p.image
                        img(:src="track.album.images[0].url")
                    p.button-bar
                        a.button.is-primary.is-large
                            span.icon(@click="selectTrack") ▶

            .column
                .panel
                    .panel-heading
                        h1.title {{ trackTitle }}
                    .panel-block
                        article.media
                            nav.level
                                .level-left
                                    a.level-item
</template>

<script>
    import trackMixin from '@/mixins/track'
    import { mapState, mapActions, mapGetters } from 'vuex'

    import PmLoader from '@/components/shared/Loader.vue'


    export default {
        name: "TrackDetail",

        mixins: [ trackMixin ],
        computed: {
            ... mapState([
                'track'
            ]),
            ... mapGetters([
                'trackTitle'
            ])
        },
        methods: {
            ... mapActions([
                'getTrackById'
            ])
        },
        components: {
            PmLoader
        },
        created() {
            const id = this.$route.params.id
            if (!this.track || !this.track.id || this.track.id !== id) {
                this.isLoading = true;
                this.getTrackById( { id } )
                    .then(() => {
                        console.log('Track loaded ...')
                    })
                this.isLoading = false
            }

        }
    }
</script>

<style scoped lang="scss">

    .columns {
        margin: 20px;
    }

    .button-bar {
        margin-top: 20px;
    }

</style>
