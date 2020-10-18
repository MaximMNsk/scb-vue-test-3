<template>
   <div class="border rounded shadow-sm p-2 result-container">
        <div class="card d-flex flex-row">
            <b-button v-b-toggle="'details-'+itemData.UUID" variant="light" class="w-100">
                <span class="font-weight-bold item-name">{{ itemData.name }}</span><br>
                <span class="font-italic"> ({{ itemData.fl }}) </span>
            </b-button>
            <b-button variant="light" class="float-right in-stored p-1" action="unsave" @click="unsaveItem">&#9733;</b-button>
        </div>
        <b-collapse :id="'details-'+itemData.UUID">
            <div class="card card-body">
                <p v-if="itemData.transcritpion">{{ itemData.transcritpion }}</p>
                <p v-for="def in itemData.descriptions" :key="def"> {{ def }} </p>
            </div>  
        </b-collapse>
    </div>
</template>




<script>

import { eventHub } from '../main';

export default {
    name: 'StoredItem',
    data(){
        return{
            isSavedFlag: Boolean,
            // debouncedEmit: null
        }
    },
    props: {
        itemData: {
            type: Object,
            default() {}
        }
    },
    methods: {
        unsaveItem(){
            let savedItems = JSON.parse(localStorage.getItem('dictItems'));
            if( savedItems ){
                let index = savedItems.findIndex(el => el.UUID === this.itemData.UUID);
                savedItems.splice(index, 1);
                this.save( savedItems );
            }
            eventHub.$emit('changeStored');
        },
        save( item ){
            try {
                localStorage.removeItem('dictItems');
                localStorage.setItem('dictItems', JSON.stringify(item));
            } catch (error) {
                localStorage.removeItem('dictItems');
                console.info(error);
            }   
        }
    },
    created(){

    }
  }
</script>




<style>
    .result-container{
        width: 300px !important;
    }
</style>