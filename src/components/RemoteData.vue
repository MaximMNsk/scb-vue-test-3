<template>
    <div class="main-container border rounded p-2 m-3 mx-auto w-75 d-flex justify-content-around">
        <div class="search-container m-2 ">
            <search-panel :searchDirection="searchDirection" @searchWord="makeSearch"></search-panel>
        </div>
        <div class="result-container m-2 ">
            {{ remoteSearchWordResult }}
        </div>
    </div>
</template>



<script>

    import SearchPanel from './SearchPanel.vue';
    import axios from 'axios';


    export default {
        name: 'RemoteData',
        components:{
            SearchPanel
        },
        props: {
            searchDirection: {
                type: Boolean,
                default(){ }
            }
        },
        data(){
            return{
                url: "https://dictionaryapi.com/api/v3/references/sd4/json/",
                apiKey: "56f99054-5a45-44ec-a5a3-06fe23eea102",
                wordCount: 10,
                remoteSearchWordResult: null
            }
        },
        methods:{
            makeSearch(word){
                axios.get(this.url+word+'?key='+this.apiKey)
                .then((response) => {
                    this.remoteSearchWordResult = response.data;
                })
                .catch(error => {
                    console.log(error);
                });            
                console.log(word);
            }
        }
    }
    
</script>


<style>
    .main-container{
        min-width: 330px;
        min-height: 350px;
    }
    .search-container{
        width: 300px !important;
    }
    .result-container{
        width: 300px !important;
    }
    @media screen and (max-width: 768px) {
        .main-container, .result-container{
            flex-direction: column;
            justify-content: center;
        }
    }
</style>