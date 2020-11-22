<template lang="pug">
#essay
    .higher
        square-title(:title='title')
        square-img(:image='image')
    .lower
        search-bar(@new-search="getResponseFromSearch($event)")
        result-list(:results="results")
        #more(v-if="answers.length") 
            button.more-search#left(v-if="prev" @click='goPrev') 
                strong &larr;
            span#center {{ resultsToSee }} RESULTS
            button.more-search#right(v-if="next" @click='goNext') 
                strong &rarr;
        p#loader(v-if="loading") Loading... Be patient, it could take some time.
        p#failure(v-if="problem") Server failed: {{ error.message }}. We are deeply sorry for the inconvenience. Can you retry later?
        p#empty(v-if="zero") Sorry, our server has not find any product. Have you another query? 
</template>

<script>
/* eslint-disable */
import SquareImg from "../components/SquareImg.vue";
import SquareTitle from "../components/SquareTitle.vue";
import SearchBar from '@/components/SearchBar.vue';
import ResultList from '@/components/ResultList.vue';
import axios from 'axios';

export default {
    data(){
        return {
            title: 'This is a page for beauty product search',
            image: '/img/beauty-products.9840c506.jpg',
            firstAnswers: [],
            results: [],
            start: 0,
            next: false,
            prev: false,
            loading: false,
            problem: false,
            zero: false
            //image: '../../resources/beauty-products.jpg',
            // /img/beauty-products.9840c506.jpg
        }
    },
    computed: {
        resultsToSee(){
            if (this.answers.length - start < 0) return 0
            return this.answers.length - start
        },
        answers(){
            return this.firstAnswers.sort( (a, b) => a.name - b.name )
        }
    },
    methods: {
        async getResponseFromSearch($event){
            this.loading = true
            await axios
                .get('https://cors-anywhere.herokuapp.com/https://skincare-api.herokuapp.com/product?q=' + $event)
                .then(res => (this.firstAnswers = res.data))
                .catch(error => {
                    this.problem = true
                    console.log(error.message)
                })
            if (!this.firstAnswers.length) this.zero = true
            else {
                this.cutTheList(this.firstAnswers, this.results, 0, 'right')
                return this.results
                } 
                
        },
        cutTheList(origin, destination, start, direction){
            this.prev = true
            this.next = true
            let i = start
            let j = direction === 'right' ? start + 5 : start - 5 
            this.start = j
            this.first = origin[0]
            this.last = origin[origin.length-1]
            if (direction === 'right') {
                while (i < j && origin[i]) {
                    destination.push(origin[i])
                    i++
                }
                if (destination.includes(this.last)) {
                    this.next = false
                }

            }
            else {
                while (i-1 > j && origin[i-1]) {
                    destination.push(origin[i-1])
                    i--
                }
                if (destination.includes(this.first)) {
                    this.prev = false
                }
            }
            return destination
        },
        sortTheList(){
            
        },
        goPrev(){
            this.results = []
            this.cutTheList(this.answers, this.results, this.start, 'left')
            return this.results
        },
        goNext(){
            this.results = []
            this.cutTheList(this.answers, this.results, this.start, 'right')
            return this.results
        }
    },
    components: {
        SquareTitle,
        SquareImg,
        SearchBar,
        ResultList
    },
    mounted(){
        //this.getResponseFromSearch('laneige')
    }
}
</script>

<style lang="stylus">
$blue = #55D7FF
$red = #DB0992
$white = #FFF 

.higher
    background $blue

    .square
        width: 100%
        display grid
        grid-template-columns repeat(4, 1fr)
        grid-template-rows repeat(3, 1fr)
    .title
        grid-column 2 / 4
        grid-row 2 / 3
        padding-top 7%
        h1
            text-align center
            font 1.4em 
            font-weight bold
    .total
        max-width 100%

.lower
    background $red
    color $white

    .more-search
        background $blue
        padding 2%
        border-radius 8%
        font-size 1em 
    #more
        grid-column 2 / 3
        grid-row 1 / 2
        display grid
        grid-template-rows repeat(1, 1fr)
        grid-template-columns repeat(7, 1fr)
        height 50%
    #left 
        grid-column 3 / 4
        grid-row 1 / 2
        width 50%
        margin 0 auto
        
    #center 
        margin-top 5px
        grid-column 4 / 5
        grid-row 1 / 2
        text-align center
        margin auto 0
    
    #right 
        grid-column 5 / 6
        grid-row 1 / 2
        width 50%
        margin 0 auto
    
    #loader
    #failure
    #empty
        margin 15px 0 0 0
        padding 0
        text-align left
        font-size 1.4em

    #form
        margin 0
        padding 0
        display grid
        grid-template-columns repeat(20, 1fr)
        grid-template-rows repeat(2, 50%)
        .search
            grid-column 1 / 16
            grid-row 2 / 3
            padding 2%
            border-radius 8%
            font-size 1.4em 
        .btn-search
            grid-column 17 / 21
            grid-row 2 / 3
            background $blue
            padding 2%
            border-radius 8%
            font-size 1.4em 
    #list
        list-style-type none
        margin 15px 0 0 0
        padding 0
        text-align left
        font-size 1.4em
        
    .result
        margin 5px 0 0 0
        font-weight bold
        span
            font-weight 200

@media (max-width: 767px) {
    #essay {
        display grid
        grid-template-rows repeat(3, 1fr)
        grid-template-columns repeat(1, 1fr)
    }
    .higher {
        grid-column: 1 / 2
        grid-row: 1 / 3
        display grid
        grid-template-rows repeat(4, 1fr)
        grid-template-columns repeat(3, 1fr)
    }
    
    .lower {
        padding 3%
        grid-column: 1 / 2
        grid-row: 3 / 4
        display grid
        grid-template-rows repeat(4, 1fr)
        grid-template-columns repeat(1, 1fr)
    }
    #loader
    #failure
    #empty
        grid-column 1 / 2
        grid-row 2 / 4
        
    #more {
        grid-column: 1 / 2
        grid-row: 4 / 5
    }
        
    
}
@media (min-width: 768px) {
    #essay {
        display grid
        grid-template-rows repeat(2, 1fr)
        grid-template-columns repeat(1, 1fr)
    }
    .higher {
        display grid
        grid-template-rows repeat(2, 1fr)
        grid-template-columns repeat(6, 1fr)
    }
    
    .lower {
        display grid
        grid-template-rows repeat(4, 1fr)
        grid-template-columns repeat(4, 1fr)
    }
    #loader
    #failure
    #empty
        grid-column 2 / 4
        grid-row 2 / 4

    #more {
        grid-column: 1 / 5
        grid-row: 4 / 5
    }

}   
</style>
