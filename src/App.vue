<template lang="pug">
    #app
        pm-header

        router-view

        pm-footer

</template>

<script>
import trackService from '@/services/track'

import PmHeader from '@/components/layout/Header.vue'
import PmFooter from "@/components/layout/Footer.vue";

export default {
    name: 'app',
    components: {
        PmHeader,
        PmFooter
    }
}
</script>

<style lang="scss">
@import "scss/main";

    .results {
        margin-top: 50px;
    }

    .is-active {
        border: 3px solid deepskyblue;
    }

</style>
