<template>
    <div class="border rounded shadow-sm p-2">
        <b-button v-b-toggle="'details-'+itemData.meta.uuid" variant="light" class="w-100">
            <span class="font-weight-bold">{{ itemData.meta.id }}</span>
            <span class="font-italic"> ({{ itemData.fl }}) </span>
        </b-button>
        <b-collapse :id="'details-'+itemData.meta.uuid">
            <div class="card card-body">
                <span class="float-right in-stored">&#9734; &#9733;</span>
                <p v-if="itemData.hwi.prs">{{ itemData.hwi.prs[0].mw }}</p>
                <p v-for="def in itemData.shortdef" :key="def"> {{ def }} </p>
            </div>  
        </b-collapse>
    </div>
</template>



<script>
export default {
    name: 'SearchedItem',
    data(){
        return{
        }
    },
    props: {
        itemData: {
            type: Object,
            default() {}
        }
    }
  }
</script>



<style>
    .in-stored{
        font-size: 150%;
    }
    .item-short-details{
        display: inline-block;
        width: 255px;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
    }
</style>