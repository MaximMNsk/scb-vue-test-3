<template>
    <div class="main-container border rounded p-2 m-3 mx-auto w-75 d-flex justify-content-around shadow">
        <div class="search-container m-2 ">
            <search-panel :searchDirection="searchDirection" :searchParams="searchParams" @searchWord="filterByWord" @addSearch="filterByFl"></search-panel>
        </div>
        <draggable v-model="finalResult" @start="drag=true" @end="drag=false">
              <stored-item v-for="resultItem in finalResult" :key="resultItem.UUID" :itemData="resultItem"></stored-item>
        </draggable>
    </div>
</template>



<script>

  import SearchPanel from './SearchPanel.vue';
  import StoredItem from './StoredItem.vue';
  import draggable from 'vuedraggable';
  import { eventHub } from '../main';

  export default {
    name: 'StoredData',
    components:{
        SearchPanel, StoredItem, draggable
    },
    props: {
        searchDirection: {
            type: Boolean,
            default(){ }
        }
    },
    data(){
      return{
        storedItems: null,
        searchParams: null,
        searchWord: null,
        searchFl: null,
        filteredByWord: Array,
        filteredByFl: Array,
        finalResult: null,
      }
    },
    methods:{
      getStoredItems(){
        let stdItems = localStorage.getItem('dictItems');
        this.storedItems = ( stdItems ) ? JSON.parse(stdItems) : null;
      },
      filterByWord(word){
        this.searchWord = word;
        const result = this.storedItems.filter(item => item.name.match(word));
        this.filteredByWord = result;
        this.finalFilter();
      },
      filterByFl(fl){
        this.searchFl = fl;
        const result = this.storedItems.filter(item => fl.join().match(item.fl));
        this.filteredByFl = result;
        this.finalFilter();
      },
      finalFilter(){
          this.filteredByWord = (this.filteredByWord.length>0) ? this.filteredByWord : this.storedItems;
          this.finalResult = this.filteredByWord.filter((val)=> this.filteredByFl.indexOf(val) != -1 );
      },
      getUniqueFl(){
        if( this.storedItems ){
          var itemsFl = [];
          for( var i=0; i<this.storedItems.length; i++ ){
            itemsFl.push(this.storedItems[i].fl);
          }
          itemsFl = [...new Set(itemsFl)];
        }
        this.searchParams = itemsFl;
      },
      reInitData(){
        this.getStoredItems();
        this.filterByWord(this.searchWord);
        this.filterByFl(this.searchFl);
        this.getUniqueFl();
      }
    },
    created(){
      this.getStoredItems();
      this.filteredByWord = this.storedItems;
      this.filteredByFl = this.storedItems;
      this.getUniqueFl();
      eventHub.$on('changeStored', () => {
        this.reInitData();
      });
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