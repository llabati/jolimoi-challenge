<template lang="pug">
#form
    input.search(type='text' name='name' placeholder="Brand, product, ingredient: type your search here" v-focus v-model="query")
    button.btn-search(type='submit' @click='send') Search
</template>

<script>
    export default {
        data(){
            return {
                query: ''
            }
        },
        methods: {
            send(){              
                console.log(this.query)
                this.$emit('new-search', this.query)
            }
        },
        directives: {
            focus(el){
                el.focus()
            }
        }
    
    } 
</script>

<style lang="stylus">
@media (max-width: 767px) {
    #form {
        grid-column: 1 / 2
        grid-row: 1 / 2
    }
    
}
@media (min-width: 768px) {
    #form {
        grid-column: 2 / 4
        grid-row: 1 / 2
    }

</style>
