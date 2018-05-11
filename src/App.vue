<template>
  <div id="app">
    <div class="imgHeader mb-4">
      <img-box-header></img-box-header>
    </div>
    <div class="container">
      <div class="row">
        <div class="col-sm-6 col-md-4 col-lg-3 col-xl-3" v-for="currentImg in currentImages">
          <img-box :img="currentImg"></img-box>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import ImageBox from './Components/ImageBox.vue'
import ImageBoxHeader from './Components/ImageBoxHeader.vue'
import $ from 'jquery'
import {eventBus} from './main.js'

export default {
  components: {
      'img-box': ImageBox,
      'img-box-header': ImageBoxHeader
  },
  data(){
      return {
        currentImages: [],
        bottom: false,
        inputSearchStr: " ",
        titleSearch: false
      }
  },
  methods: {
    bottomVisible() {
      const scrollY = window.scrollY;
      const visible = document.documentElement.clientHeight;
      const pageHeight = document.documentElement.scrollHeight;
      const bottomOfPage = visible + scrollY >= pageHeight;
      return bottomOfPage || pageHeight < visible
    },

    addMoreImgs(){

      // IF THERE ARE NO PHOTOS ON THE SCREEN IS BECAUSE MY API KEY IS EXPIRED. THIS HAPPENED ONCE WHILE I WAS DEVELOPING THE APP AND I HAD TO MAKE MYSELF A NEW API KEY.
      let defaultUrl = "https://api.flickr.com/services/rest/?method=flickr.photos.search&api_key=a779e9b7d9a5c0be99a4d19e541494ed&tags=" + this.inputSearchStr + "&per_page=4&format=json&nojsoncallback=1";
      let searchByTitleAndTagsUrl = "https://api.flickr.com/services/rest/?method=flickr.photos.search&api_key=a779e9b7d9a5c0be99a4d19e541494ed&text=" + this.inputSearchStr + "&per_page=4&format=json&nojsoncallback=1";

      if(this.titleSearch){
          defaultUrl = searchByTitleAndTagsUrl;
      }

      $.ajax({
        url: defaultUrl,
        dataType: 'json'
      }).then((data) => {

        for(let i = 0; i < data.photos.photo.length; i++){

          let currentImgId = data.photos.photo[i].id;

          $.ajax({
            url: 'https://api.flickr.com/services/rest/?method=flickr.photos.getInfo&api_key=a779e9b7d9a5c0be99a4d19e541494ed&photo_id=' + currentImgId + '&format=json&nojsoncallback=1',
            dataType: 'json'
          }).then((data) => {
            this.currentImages.push(data);
          });
        }
      })
    }
  },
  watch: {
    bottom(value){
        if(value){
            this.addMoreImgs();
        }
    },
    inputSearchStr(){
      this.currentImages = [];
      this.addMoreImgs();
    }
  },
  created(){
    window.addEventListener('scroll', () => {
      this.bottom = this.bottomVisible()
    });

    this.addMoreImgs();

    eventBus.$on('inputStringChanged',(data) => {

      if(data.trim().length > 0){
          this.inputSearchStr = data;
      }
      else{
        this.inputSearchStr = " ";
      }
    });

    eventBus.$on('titleSearchTurned',(data) => {
      this.titleSearch = data;
    });
  }
}
</script>
