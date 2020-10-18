<template>
    <div>
        <div class="input-group mb-3">
            <input type="text" class="form-control" placeholder="Type a word" v-model="typeWord">
            <div class="input-group-prepend">
                <span class="input-group-text">&#128269;</span>
            </div>
        </div>
        <b-form-checkbox-group id="checkbox-group" v-model="checkFl" name="checkFl" class="d-flex flex-column">
            <b-form-checkbox v-for="param in searchParams" :value="param" :key="param" class="add-params">{{ param }}</b-form-checkbox>
        </b-form-checkbox-group>
    </div>
</template>



<script>

  export default {
    name: 'SearchPanel',
    data(){
        return{
            typeWord: "",
            checkFl: [],
        }
    },
    props: {
        searchDirection: {
            type: Boolean,
            default(){}
        },
        searchParams: {
            default(){}
        }
    },
    watch: {
        typeWord: function(){
            this.$emit("searchWord", this.typeWord);
        },
        checkFl: function(){
            this.$emit("addSearch", this.checkFl);
        }
    },
    created(){
        this.checkFl = this.searchParams;
    }
  }

</script>


<style>
    .add-params{
        color: grey;
    }
</style>