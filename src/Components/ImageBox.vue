<template>
  <div class="contianer">
    <div class="card">
      <div class="card-header text-center">
        <img :src="getSrcLink" class="rounded img-fluid" :title="img.photo.title._content">
        <div class="mt-4" id="img-title">
          <a :href="img.photo.urls.url[0]._content" class="h5" v-if="img.photo.title._content.trim().length > 0">{{img.photo.title._content}}</a>
          <a :href="img.photo.urls.url[0]._content" class="h5" v-else>Unknown Image Title</a>
        </div>
        <div class="mt-2">
          by <p><a :href="authorLink">{{img.photo.owner.username}}</a></p>
        </div>
      </div>
      <div class="card-body">
        <p v-html="description"></p>
      </div>
      <div class="card-footer">
        <span>Tags:</span> <a href="#" v-for="tag in tags">{{tag}} </a>
      </div>
    </div>
  </div>
</template>

<script>
  import $ from 'jquery'

  export default {
      props:['img'],
      computed: {
          getSrcLink(){
            let srcLinkOfCurrentImg = "https://farm" + this.img.photo.farm + ".staticflickr.com/" + this.img.photo.server + "/" + this.img.photo.id + "_" + this.img.photo.secret + ".jpg";

            return srcLinkOfCurrentImg;
          },
          tags(){
              let tagArray = [];

              for(let i = 0; i < this.img.photo.tags.tag.length; i++){
                let currentTag = this.img.photo.tags.tag[i]._content;
                tagArray.push(currentTag);
              }

              return tagArray;
          },
          authorLink(){
              return this.img.photo.urls.url[0]._content.split('/').slice(0, -2).join('/')
          },
          description(){
            const fullContent = this.img.photo.description._content;
            let content = fullContent;

            if(content.trim().length > 0){
                return content;
            }
            else{
                return "This Photo Doesn't Have Description"
            }
          }
      }
  }
</script>

<style lang="scss" scoped>
  .container {
    .card {
      .card-header {

        height: 350px;

        #img-title{
          white-space: nowrap;
          overflow:hidden !important;
          text-overflow: ellipsis;
        }

        img {
          max-width: 90%;
          height: 200px;
          width: auto;
        }
      }
    }
  }
</style>
