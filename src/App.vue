<template>
    <h1><b>Vue App for searching Images</b></h1>
    <div class="inputs">
        <input class="Input1" type="text" v-model="searchQuery" />
        <button @click="fetchSearchPhoto">Search images</button>
        <button @click="fetchPrevPage">prev page</button>
        <button @click="fetchNextPage">next page</button>
        <button @click="printPdf">Save Slides to Pdf </button>
    </div>
        <div class="reveal" id="presentation">
                <div class="slides" :revealjs_autoslide="1000">
                    <!-- <section><h1>Images for : {{this.searchQuery}}</h1></section>  -->
                    <section v-for="num in imageArray" :key="num">
                        <div>
                            <img :src="num.urls.regular" width="600" height="500"/>
                            <p v-for="tag in num.tags" :key="tag">{{tag.title}}</p>
                            <h4>Created on: {{num.created_at}}</h4>
                        </div>
                    </section>      
            </div>

        </div>
</template>

<script>
import Reveal from "reveal.js";
//import Markdown from "reveal.js/plugin/markdown/markdown.js";
//import Highlight from "reveal.js/plugin/highlight/highlight.js";
import "reveal.js/dist/reveal.css";
// import "reveal.js/dist/theme/black.css";
import "reveal.js/plugin/highlight/monokai.css";

export default {
    name:'app',
    data(){
        return {
            accessKey: 'FE7uoOOR8-w_89OBEXrNXh-zDDcU6CfmOA-q1O_H3CA',
            url :'https://api.unsplash.com/search/photos',
            image:'',
            searchQuery:'',
            imageArray: [],
            pageNum: 1,
            slidesArray: []
        };
    },
    methods: {
        fetchSearchPhoto(){
            //console.log('queary value',this.searchQuery);

            if(this.searchQuery !== ''){
            fetch(this.url + '?page='+this.pageNum+'&query='+ this.searchQuery +'&client_id=' + this.accessKey)
            //fetch('https://api.unsplash.com/search/photos?page=1&query=office&client_id=' + this.accessKey)
            .then(
                (response)=>response.json())
            .then((json)=> {
                this.imageArray = json.results;
            })
            .catch((err)=>{
                console.log('error',err)
            })
            }
        },
        fetchPrevPage() {
            if (this.pageNum > 1){
                this.pageNum = this.pageNum - 1;
            }
            this.fetchSearchPhoto();
        },
        fetchNextPage() {
            this.pageNum = this.pageNum + 1;
            this.fetchSearchPhoto();
        },
        printPdf(){
            window.print();
        }
    },
    mounted(){
        Reveal.initialize({
            controls: true,
        });
    },
    updated(){
        Reveal.configure({controls: true});
        Reveal.slide(0, 1);
    },
};
</script>

<style>
img {
    border-radius: 8px;
    display: block;
    margin-left: auto;
    margin-right: auto;
}
.tags{
    font-size: 18pt;
    font: bold;
    width: 30%;
    border: 2px solid blueviolet;

    display: block;
    margin-left: auto;
    margin-right: auto;

    text-align: center;
    padding: 10px 10px;
    margin-bottom: 50px;
}
#app{
    align-items:center;
    /* margin: 5rem 30rem; */
}
button {
    font-size: x-large;
    padding: 12px 20px;
    box-sizing: border-box;
}
input {
    font-size: 20pt;
    padding: 12px 20px;
    box-sizing: border-box;
}

/* #app { */
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100vh;
}
</style>
