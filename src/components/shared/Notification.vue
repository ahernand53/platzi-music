<template lang="pug">

    .container
        .columns
            .column.is-4.is-offset-4
                .notification(:class="type")
                    slot(name="body") Algo anduvo mal


</template>

<script>
    export default {
        name: 'PmNotification',
        props: {
            type: {
                type: String,
                default: 'is-danger'
            }
        }
    }
</script>

<style scoped lang="scss">
    .notification {
        margin: 10px;
    }
</style>
