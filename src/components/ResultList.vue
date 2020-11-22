<template lang="pug">

ul#list
    li(v-for="item in results")
        result-item(:item="item") 
      
</template>

<script>
import ResultItem from './ResultItem';
export default {
    props: {
        results: Array
    },
    components: {
        ResultItem
    }
}
</script>

<style lang="stylus">
@media (max-width: 767px) {
    #list {
        grid-column: 1 / 2
        grid-row: 2 / 4
    } 
}
@media (min-width: 768px) {
    #list {
        grid-column: 2 / 5
        grid-row: 2 / 4
    }  
}  
</style>
