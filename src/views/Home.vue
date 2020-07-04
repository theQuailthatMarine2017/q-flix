<template>
  <div class="home">


    <v-app-bar style="background:#333333;background:-webkit-linear-gradient(to left,#333333,#dd1818);background: linear-gradient(to left, #333333, #dd1818);">
      <v-toolbar-title style="color:white;font-family:courier;" class="display-1 font-weight-light ml-0 pl-4">
        <span>Q-Flixs. <span class="headline font-weight-light">Endless Entertainment</span></span>
      </v-toolbar-title>
      <v-spacer />
      
     
    </v-app-bar>

    <v-container grid-list-md text-xs-center>

      <swiper ref="mySwiper" :options="swiperOptions">
    <swiper-slide>Slide 1</swiper-slide>
    <swiper-slide>Slide 2</swiper-slide>
    <swiper-slide>Slide 3</swiper-slide>
    <swiper-slide>Slide 4</swiper-slide>
    <swiper-slide>Slide 5</swiper-slide>
    <div class="swiper-button-prev" slot="button-prev"></div>
        <div class="swiper-button-next" slot="button-next"></div>
  </swiper>


    <v-layout row >

       <v-flex xl3 xs6 lg3 @click="getmovie(movie._id)" v-for="movie in movies" :key="movie._id">
         <v-hover>
    <template v-slot:default="{ hover }">
            <v-card>
            <img :src="movie.imgPath" height="400px" width="100%">
             <v-fade-transition>
          <v-overlay
            v-if="hover"
            absolute
            color="#8b0000"
          >
          <div align="center">
            <p style="color:white" class="title">{{ movie.title }}</p>
            <p style="color:white" class="subtitle">{{ movie.description }}</p>
          </div>
          </v-overlay>
        </v-fade-transition>
          </v-card>
    </template>
         </v-hover>
      </v-flex>


    </v-layout>


    </v-container>


    <v-dialog persistent v-model="movieplay">
      <v-toolbar dark color="#8b0000">
      <v-btn icon dark @click="stopmovie">
            <v-icon>mdi-close</v-icon>
          </v-btn>
          <v-toolbar-title>{{ movie_title }}</v-toolbar-title>
      </v-toolbar>
      <video class="video-size"  controls>
  <source :src="movie_source" type="video/mp4">
</video>
    </v-dialog>


  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios';


export default {
  name:'carrousel',
    data(){
      return{
        swiperOptions : {
        slidesPerView: 5,
        spaceBetween: 0,
        freeMode: true,
        loop: true,
        navigation: {
          nextEl: '.swiper-button-next',
          prevEl: '.swiper-button-prev'
        }
      },
        movieplay:false,
        isComponentModalActive:false,
        movie_source:'',
        movie_title:'',
        movies:null,
      }
    },
    
    mounted(){

        this.getmovies()

        console.log('Current Swiper instance object', this.swiper)
        this.swiper.slideTo(3, 1000, false)
    },
    methods:{
        getmovies(){

            //this.isLoading = true;

            axios.get('http://62.8.71.129:8500/api/q-flix/movies').then( response => {

              this.movies =  response.data
              //this.isLoading = false
              console.log("Movies" + this.movies[0])

            })
            
        },
        getmovie(id){


          this.$router.push({ name:'PlayMovie', query: { watch: id }})

        },
        stopmovie(){

          this.movie_source = '',
          this.movieplay = false
        }
    },
     computed: {
      swiper() {
        return this.$refs.mySwiper.$swiper
      }
    }
}
</script>

<style scoped>

.img-size{
  max-height:350px;
  max-width:190px;
}

.home{
  background: #333333;
  background: -webkit-linear-gradient(to bottom, #333333, #dd1818);;
  background: linear-gradient(to bottom, #333333, #dd1818);
  height:100%;
}

.video-size{
  width:100%;
  height:98%;
  
}
</style>
