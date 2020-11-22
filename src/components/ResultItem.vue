<template lang="pug">
p.result
    strong &bull; {{ item.name }} -
    span  {{ item.brand }}
</template>

<script>
export default {
    props: {
        item: Object
    },
}
</script>

<style lang="stylus">

</style>
