<template>
  <div id="main">
    <div class="container">
      <div class="row justify-content-end">
        <div class="d-flex flex-column mt-4 mr-4">
          <div>
            <input type="checkbox" v-model="liveSearchBool" id="liveSearch">
            <label for="liveSearch">Live Search - <span class="text-success" v-if="liveSearchBool">On</span><span class="text-danger" v-else>Off</span></label>
          </div>
          <div>
            <input type="checkbox" v-model="titleSearchBool" id="titleSearch">
            <label for="liveSearch">Title Search - <span class="text-success" v-if="titleSearchBool">On</span><span class="text-danger" v-else>Off</span></label>
          </div>
        </div>
        <div class="col-6-xl col-6-lg mt-4">
          <button type="button" class="btn btn-warning btn-sm mr-1 mb-1" @click.prevent="search">Search</button> <input type="text" v-model="searchString" id="searchField"> <i class="fa fa-search mr-2"></i>
          <p class="text-primary">Search By: <span class="text-danger">Tags</span><span class="text-danger" v-if="titleSearchBool">, Title</span></p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

  import {eventBus} from '.././main.js'

  export default {
      data(){
          return {
              liveSearchBool: false,
              titleSearchBool: false,
              searchString: ""
          }
      },
      methods: {
        search(){
          eventBus.$emit('inputStringChanged', this.searchString);
        }
      },
      watch: {
          searchString(value){
            if(this.liveSearchBool){
              eventBus.$emit('inputStringChanged', value);
            }
          },
          titleSearchBool(value){
            eventBus.$emit('titleSearchTurned', value);
          }
      }
  }
</script>

<style lang="scss" scoped>
  #main{
    border-bottom: 4px solid #dbdbdb;
    background-color: #e9e9e9;
    .container{
      .row{
        /*height: 110px;*/

        button {
          height: 38%;
        }

        #searchField {
          width: 50%;
          height: 34%;
          border-top: 2px solid #dbdbdb;
        }

        i {
          color: gray;
        }
      }
    }
  }
</style>
