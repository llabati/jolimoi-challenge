<template lang="pug">
p.result
    strong &bull; {{ item.brand }} -
    span  {{ item.name }}
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
