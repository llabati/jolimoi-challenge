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
            span#center {{ resultsToSee }} RESULTS LEFT TO SEE
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
            title: 'This is a page for\nbeauty product search',
            //image: '../assets/beauty-products.jpg',
            image: '/img/beauty-products.9840c506.jpg',
            firstAnswers: [],
            results: [],
            start: 0,
            next: false,
            prev: false,
            loading: false,
            problem: false,
            zero: false
        }
    },
    computed: {
        resultsToSee(){
            if (this.answers.length - this.start < 0) return 0
            return this.answers.length - this.start
        },
        answers(){
            return this.firstAnswers.sort( (a, b) => a.name - b.name )
        },
        first(){
            return this.answers[0]
        },
        last(){
            return this.answers[this.answers.length-1]
        }
    },
    methods: {
        // make the API call
        async getResponseFromSearch($event){
            this.results = []
            this.zero = false
            this.problem = false
            this.loading = true
            await axios
                .get('https://cors-anywhere.herokuapp.com/https://skincare-api.herokuapp.com/product?q=' + $event)
                .then(res => (this.firstAnswers = res.data))
                .catch(error => {
                    this.problem = true
                    console.log(error.message)
                })
            this.loading = false
            
            // prepare the display of the results or the error
            if (!this.firstAnswers.length) this.zero = true
            else {
                this.cutTheList('right')
                return this.results
                } 
                
        },
        
        // prepare arrays of 5 items 
        cutTheList(direction){
            this.prev = true
            this.next = true
            let i = this.start 
            
            if (direction === 'right') {
                let j = this.answers.length < this.start + 5 ? this.answers.length : this.start + 5
                this.start = j
                while (i < j && this.answers[i]) {
                    this.results.push(this.answers[i])
                    i++
                }
            }
            else {
                let j = this.start - 5 < 0 ? 0 : this.start - 5
                this.start = j
                --i
                while (i >= j && this.answers[i]) {
                    this.results.push(this.answers[i])
                    i--
                }
            }
            if (this.results.includes(this.first)) {
                    this.prev = false
                }
                if (this.results.includes(this.last)) {
                    this.next = false
                }
            return this.results
        },
        
        goPrev(){
            this.results = []
            this.cutTheList('left')
            return this.results
        },
        goNext(){
            this.results = []
            this.cutTheList('right')
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
            font 1.8em 
            font-weight bold
            white-space break-spaces
    .total
        max-width 100%

.lower
    background $red
    color $white

    #more
        grid-column 2 / 3
        grid-row 1 / 2
        display grid
        grid-template-rows repeat(1, 1fr)
        grid-template-columns repeat(7, 1fr)
        margin-top 15px
        height 50%
        
    .more-search
        background $blue
        padding 2%
        border-radius 8%
        font-size 1em 
    
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
        animation fade .5s ease

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
        margin 0
        padding 0
        font-size 1.4em
        font-weight bold
        height 1.1em
        animation enter .5s ease
        
        span
            font-weight 200
            
@keyframes fade {
    from {
       opacity 1
    }
    to {
       opacity 0 
    }
}
@keyframes enter {
    from {
       opacity 0 
    }
    to {
       opacity 1  
    }
}

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
