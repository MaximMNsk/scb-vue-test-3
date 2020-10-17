<template>
    <div class="border rounded shadow-sm p-2">
        <div class="card d-flex flex-row">
            <b-button v-b-toggle="'details-'+itemData.meta.uuid" variant="light" class="w-100">
                <span class="font-weight-bold item-name">{{ itemData.meta.id }}</span><br>
                <span class="font-italic"> ({{ itemData.fl }}) </span>
            </b-button>
            <b-button variant="light" class="float-right in-stored p-1" action="save" v-if="!isSavedFlag" @click="saveItem">&#9734;</b-button>
            <b-button variant="light" class="float-right in-stored p-1" action="unsave" v-else @click="unsaveItem">&#9733;</b-button>
        </div>
        <b-collapse :id="'details-'+itemData.meta.uuid">
            <div class="card card-body">
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
            isSavedFlag: false
        }
    },
    props: {
        itemData: {
            type: Object,
            default() {}
        },
        storedItems: {
            // type: Object,
            default() {}
        }
    },
    methods: {
        saveItem(){
            // localStorage.removeItem('dictItems');
            let transcr = (this.itemData.hwi.prs) ? this.itemData.hwi.prs[0].mw : null;
            let savedItems = JSON.parse(localStorage.getItem('dictItems'));
            let itemToSave = {
                UUID: this.itemData.meta.uuid,
                name: this.itemData.meta.id,
                fl: this.itemData.fl,
                transcritpion:  transcr,
                descriptions: this.itemData.shortdef
            };

            let itemsToSave = [];
            (savedItems) ? itemsToSave = savedItems : null;
            itemsToSave.push(itemToSave);

            this.save( itemsToSave );
            this.toggleFlag();
        },
        unsaveItem(){
            let savedItems = JSON.parse(localStorage.getItem('dictItems'));
            if( savedItems ){
                let index = savedItems.findIndex(el => el.UUID === this.itemData.meta.uuid);
                savedItems.splice(index, 1);
                this.save( savedItems );
                this.toggleFlag();
            }
        },
        toggleFlag(){
            return this.isSavedFlag = !this.isSavedFlag;
        },
        save( item ){
            try {
                localStorage.removeItem('dictItems');
                localStorage.setItem('dictItems', JSON.stringify(item));
                console.info(item);
                // this.check();
            } catch (error) {
                localStorage.removeItem('dictItems');
                console.info(error);
            }        
        }
    },
    created() {
            let currentItemUUID = this.itemData.meta.uuid;
            if( this.storedItems ){
                let obj = this.storedItems.find(item => item.UUID === currentItemUUID);
                if( obj ){
                    this.toggleFlag();
                }

            }
    }
}
</script>



<style>
    .in-stored{
        font-size: 150% !important;
        width: 30px;
    }
    .item-name{
        width: 200px;
        display: inline-block;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
    }
</style>