<template>
  <div class="video">



    <v-app-bar style="background:#333333;background:-webkit-linear-gradient(to left,#333333,#dd1818);background: linear-gradient(to left, #333333, #dd1818);">
      <span id="siteseal"><script async type="text/javascript" src="https://seal.godaddy.com/getSeal?sealID=u35SBrmeAVEoxUxEeeOiqRsTKbqEFgNYXGb7Udl0Hp2WDUUvG0WG3eM3wIGc"></script></span>
      <v-toolbar-title style="color:white;" class="display-1 font-weight-light ml-0 pl-4">
        <span>Q-Flixs. <span class="headline font-weight-light">Endless Entertainment</span></span>
      </v-toolbar-title>
      <v-spacer />
    </v-app-bar>

    <v-container v-if="movie_source != ''">
        <video class="video-size"  controls>
            <source :src="movie_source" type="video/mp4">
            </video>
    </v-container>
  </div>
</template>

<script>
import axios from 'axios';



export default {
  mounted(){

    this.play()
  },
    data(){
        return{
          movie_source:'',
          isLoading: false,
          fullPage: true
        }
    },
    
    methods:{
      play(){

        axios.get('http://62.8.71.129:8500/api/q-flix/get_movie/'+this.$route.query.watch).then(response => {

             this.movie_source = "http://62.8.71.129:8500/api/q-flix/play/"+response.data[0]._id
             console.log(this.movie_source)


          })

      }
    }
}
</script>

<style scoped>

.video{
  height:100%;
}

.video-size{
  width:100%;
  height:100%;
  
}
</style>