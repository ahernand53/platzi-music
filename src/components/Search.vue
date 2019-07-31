<template lang="pug">
    main
        transition(name="move")
            pm-notification(v-show="showNotification" :type="typeNotification")
                p(slot="body") {{ messageNotification }}

        transition(name="move")
            pm-loader(v-show="isLoading")

        section.section(v-show="!isLoading")
            nav.nav.has-shadow
                .container
                    input.input.is-large(
                        type="text",
                        placeholder="Buscar Canciones",
                        v-model="searchQuery",
                        @keyup.enter="search"
                    )
                    a.button.is-info.is-large(@click="search") Buscar
                    a.button.is-danger.is-large &times
            .container
                p
                    small {{ searchMessage }}

            .container.results
                transition-group(name="fade", tag="div", class="columns is-multiline")
                    .column.is-one-quarter(v-for="track in tracks", :key="track.id")
                        pm-track(
                            v-blur="track.preview_url",
                            :class="{'is-active' : track.id === trackIdSelected}",
                            :track="track"
                            @select="setSelectedTrack"
                        )


</template>

<script>
import trackService from '@/services/track'

import PmTrack from "@/components/Track";

import PmLoader from '@/components/shared/Loader.vue'
import PmNotification from '@/components/shared/Notification.vue'

export default {
    name: 'app',
    data () {
        return {
            searchQuery: '',
            tracks: [],

            isLoading: false,
            showNotification: false,

            typeNotification: '',
            messageNotification: '',

            trackIdSelected: null
        }
    },
    methods: {
        search() {
            if (!this.searchQuery) { return }
            this.isLoading = true;

            trackService.search(this.searchQuery)
                .then(res => {
                    this.tracks = res.tracks.items
                    this.isLoading = false

                    var total = res.tracks.total
                    if (total === 0) {
                        this.updateNotification(
                            'is-danger',
                            'No se encontro ninguna cancion'
                        )
                    } else {
                        this.updateNotification(
                            'is-info',
                            `Se encontraron ${total} canciones`
                        )
                    }
                })
                .catch((error) => {
                    this.updateNotification(
                        'is-danger',
                        'Ocurrio un error externo'
                    )
                  console.log(error)
                    this.isLoading = false
                })
        },
        updateNotification(type, message) {
            this.typeNotification = type
            this.messageNotification = message
            this.showNotification = true
        },
        setSelectedTrack(trackId) {
            this.trackIdSelected = trackId
        }
    },
    computed: {
        searchMessage() {
            return `Encontrados: ${this.tracks.length}`
        }
    },
    watch: {
        showNotification () {
            if (this.showNotification) {
                setTimeout(() => {
                    this.showNotification = false
                }, 3000)
            }
        }
    },
    components: {
        PmTrack,
        PmLoader,
        PmNotification
    }
}
</script>

<style lang="scss">
@import "~bulma/bulma";

    .results {
        margin-top: 50px;
    }

    .is-active {
        border: 3px solid deepskyblue;
    }

</style>
