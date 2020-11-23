# Front-End Developer Interview Challenge

## Framework
I used Vue.js 2, following your base. 

My main component is Essay.vue, which handles API calls and feeds props to its child components.

## Two problems : 
- I couldn't load the Effra font on github, probably due to copyright issues. I replaced it with the PT Sans font.
- I wanted to transmit in prop the source of the "beauty-products" image. but this one is coded, it only works on localhost/8080 and 8083. I put in comment the hard-coded src if needed. 

## HTML and CSS
For the HTML, I used Pug, for its readability.

For CSS, I worked with Stylus. But this one caused a crash of the application, probably because of my attempt to install Jest in it. Since then, even without Jest, the console receives many warnings and error messages from Stylus. In case of problem, open the application on the repo /llabati/jolimoi-challenge-css. There is only classic CSS.

I used the GRID module for all element placement. I find it is the safest and simplest solution to make sure that everything stays in the right place, even in a "responsive" situation.

## API call
I used AXIOS, which remains the most obvious solution. 

Since the answers usually consist of more than 10 items, I added navigation buttons to your layout, as well as a display of 5 results (not 4).


