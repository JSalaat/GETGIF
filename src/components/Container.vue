<template>
  <div class="hello">
    <center style="margin-bottom: 10px;">Designed with <b>V</b> & ♥️ by <b>Junaid Salaat</b></center>

    <b-container class="bv-example-row">

      <b-jumbotron>
        <b-row class="">
          <b-col cols="6">
            <h2>{{msg}}</h2>
            <p>Get your Favorite GIF downloaded here</p>

            <b-form-row class="">
              <b-col cols="10">
                <b-form-input v-model="text" type="search" v-on:keydown="12" placeholder="Enter your name"></b-form-input>
              </b-col>
              <b-col cols="1" class="search">
                <b-button variant="primary" class="search-trigger" href="#" v-on:click="search">
                  <i class="carousel-control-next-icon"></i>
                </b-button>
              </b-col>
            </b-form-row>

          </b-col>
          <b-col cols="6" class="text-center">
            <b-img :src="require('../assets/batman.gif')" class="batman" fluid alt="Responsive image"></b-img>
          </b-col>
        </b-row>
      </b-jumbotron>

      <b-container>
        <b-row v-if="gifData.length">
          <div>
            <b-card-group columns>
              <b-card v-bind:key=gif.id
                      v-for="gif in gifData"
                      :title=gif.username
                      :img-src=gif.images.preview_webp.url
                      :img-alt=gif.title
                      img-top>
                <b-card-text>
                  {{gif.title}}
                  <br>
                </b-card-text>
                <b-card-footer class="text-right">
                  <b-button :href="gif.images.original.webp" variant="outline-primary" size="sm"> GIF</b-button>
                  <b-button :href="gif.images.original.mp4" variant="outline-primary" size="sm"> MP4</b-button>
                </b-card-footer>
              </b-card>
            </b-card-group>
          </div>

          <b-button block variant="primary" v-on:click="loadMore">Load More!</b-button>


        </b-row>
        <div v-else class="text-center">
          <h3>Try Searching! 🤩</h3>
        </div>

      </b-container>

    </b-container>
  </div>
</template>

<script>
  export default {
    name: 'Container',
    data() {
      return {
        text: '',
        gifData: [],
        offset: 0,
      }
    },
    props: {
      msg: String
    },
    methods: {
      search: function (loadmore) {
        if (!loadmore) {
          this.offset = 0;
          this.gifData = [];
        }
        let key = 'd5saQxFfxlNDCg6eLwC7Md9Kp79AoklI';
        let limit = 10;
        fetch(`http://api.giphy.com/v1/gifs/search?api_key=${key}&q=${this.text}&limit=${limit}&offset=${this.offset}`)
                .then((response) => response.json())
                .then((res)=>{
                  // window.console.log(this.gifData);
                  // this.gifData = res.data;
                  // this.gifData = JSON.parse(JSON.stringify(res.data));
                  this.gifData = [...this.gifData, ...res.data];

                  // window.console.log(res.data);
                })
                .catch((err) => window.console.error(err))
      },
      loadMore: function () {
        this.offset = this.offset + 10;
        this.search(true)
      }
    }
  }
</script>

<style scoped>
  h3 {
    margin: 40px 0 0;
  }
  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    display: inline-block;
    margin: 0 10px;
  }
  a {
    color: #42b983;
  }
  .batman{
    height: 200px;
  }
  .search{
    text-align: left;
  }
  .search-trigger{
    height: 38px;
  }

</style>
