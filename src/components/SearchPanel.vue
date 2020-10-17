<template>
    <div>
        <div class="input-group mb-3">
            <input type="text" class="form-control" placeholder="Type a word" v-model="typeWord">
            <div class="input-group-prepend">
                <span class="input-group-text">&#128269;</span>
            </div>
        </div>
        <div v-if="!searchDirection">Additional parameters:</div>
    </div>
</template>



<script>

  export default {
    name: 'SearchPanel',
    data(){
        return{
            typeWord: ""
        }
    },
    props: {
        searchDirection: {
            type: Boolean,
            default(){}
        }
    },
    watch: {
        typeWord: function(){
            this.$emit("searchWord", this.typeWord);
        }
    }
  }

</script>


<style>

</style>