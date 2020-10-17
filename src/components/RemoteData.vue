<template>
    <div class="main-container border rounded p-2 m-3 mx-auto w-75 d-flex justify-content-around shadow">
        <div class="search-container m-2 ">
            <search-panel :searchDirection="searchDirection" @searchWord="makeSearch"></search-panel>
        </div>
        <div class="result-container m-2 ">
                <searched-item v-for="resultItem in finalResult" :key="resultItem.meta.uuid" :itemData="resultItem" :storedItems="storedItems"></searched-item>
        </div>
    </div>
</template>



<script>

    import SearchPanel from './SearchPanel.vue';
    import SearchedItem from './SearchedItem.vue';
    import axios from 'axios';
    import {debounce} from 'vue-debounce';

    export default {
        name: 'RemoteData',
        components:{
            SearchPanel, SearchedItem
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
                remoteSearchWordResult: null,
                finalResult: null,
                debouncedParseRemoteData: Function,
                storedItems: null
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
                // localStorage.removeItem('dictItems');
                let stdItems = localStorage.getItem('dictItems');
                // console.info(storedItems);
                this.storedItems = ( stdItems ) ? JSON.parse(stdItems) : null;
                this.debouncedParseRemoteData(word);
            },
            parseRemoteData(remoteSearchWord) {
                if(this.remoteSearchWordResult && this.remoteSearchWordResult.length>0 && this.remoteSearchWordResult[0].meta){
                    this.finalResult = (remoteSearchWord.length>0) ? this.sortAndTrimData(this.remoteSearchWordResult) : null;
                }
            },
            sortAndTrimData( arrObj ) {
                var sortedArr = arrObj.sort((a,b) => a.meta.id > b.meta.id ? 1 : -1);
                return sortedArr.slice(0, this.wordCount);
            }
        },
        created: function()  {
            this.debouncedParseRemoteData = debounce(this.parseRemoteData, 300);
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