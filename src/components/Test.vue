<template>
  <div class="movie">
    <div class="header">
      <img v-on:click="watch($event)" src="../assets/youtube_logo.png" class="trailer-btn"/>

      <h1 class="In-Theaters">{{title}}</h1>
      <h2 class="Top-Movies-This-Week">{{subTitle}}</h2>
    </div>
    <div class="view-more">
    </div>
    <swiper :options="swiperOption">
      <swiper-slide v-for="movie in movies">
        <div class="left-info" v-bind:id="movie.id">
          <span class="One-line-movie-title">{{movie.title}} | {{movie.release_date}}</span>
          <p>
            <span class="desc">{{movie.overview}}</span>
          </p>
          <div class="star-ratings-sprite">
            <span :style="{width: movie.vote_average*10+'%'}" class="star-ratings-sprite-rating"></span>
          </div>
        </div>
        <div class="right-info" :style="{backgroundImage: 'url(https://image.tmdb.org/t/p/w500/' + movie.poster_path + ')'}">
        </div>
      </swiper-slide>
      <div class="swiper-pagination"  slot="pagination"></div>
    </swiper>
  </div>
</template>



<script>
  const axios = require('axios');
  export default {
    name: 'movie',
    data(){
      return{
        title: "In Theaters",
        subTitle: "Upcoming Movies",
        movies:[],
        swiperOption: {
          autoplay: 0,
          setWrapperSize :true,
          pagination : '.swiper-pagination',
          paginationClickable :true,
          mousewheelControl : true,
          observeParents:true,
          initialSlide :0,
          keyboardControl : true,
          loop : true,
          preventClicks : false,
        },
      }
    },
    created: function(){
        var self=this;
          axios.get('https://api.themoviedb.org/3/movie/upcoming?api_key=f7c5bb6cab4de28f80a47e606646dd5c&language=en-US&page=1')
            .then(function(response){
              Array.prototype.push.apply(self.movies, response.data.results);
              self.movies.sort()
            })
            .catch(function(error){
              console.log(error);
            });
    },
    methods: {
      watch: function (event) {
        event.preventDefault();
        const ID = document.getElementsByClassName('swiper-slide-active')["0"].firstChild.id;
        axios.get('https://api.themoviedb.org/3/movie/'+ID+'/videos?api_key=f7c5bb6cab4de28f80a47e606646dd5c&language=en-US')
          .then(function(response){
            window.location.href = "https://www.youtube.com/watch?v="+response.data.results[0].key
          })
          .catch(function(error){
            console.log(error);
          });
      }

  }


  }
</script>



<style scoped>
  .trailer-btn {
    width: 100px;
  }

  .In-Theaters {
    font-family: Roboto;
    font-size: 24px;
    font-weight: 300;
    font-style: normal;
    font-stretch: normal;
    color: #3b3f47;
    text-align: left;
    margin: 0;
  }
  .Top-Movies-This-Week {
    font-family: Roboto;
    font-size: 14px;
    font-weight: normal;
    font-style: normal;
    font-stretch: normal;
    letter-spacing: 0.5px;
    color: #6b6b6b;
    text-align: left;
  }
  .One-line-movie-title {
    width: 206px;
    height: 20px;
    font-family: Roboto;
    font-size: 16px;
    font-weight: normal;
    font-style: normal;
    font-stretch: normal;
    line-height: 1.25;
    letter-spacing: 0.4px;
    text-align: left;

  }
  .desc{
    font-family: Roboto;
    font-size: 14px;
    font-weight: normal;
    font-style: normal;
    font-stretch: normal;
    line-height: 1.29;
    letter-spacing: 0.5px;
    text-align: left;
    color: #6b6b6b;
    text-align: left;

  }
  #view-more-link{
    font-family: Roboto;
    font-size: 14px;
    font-weight: 300;
    font-style: normal;
    font-stretch: normal;
    letter-spacing: 0.5px;
    text-align: right;
    color: #276cf2;
    text-decoration: none;
  }
  .view-more{
    position: absolute;
    top: 10%;
    right: 6%;
  }

  ul{
    padding-left: 0;
  }
  ul li{
    list-style-type: none;
  }
  .movie-container ul li{
    display: flex;
    justify-content: center;
    width: 100%;
  }
  .left-info{
    text-align: left;
    flex:2;
    width:48%;
    background-color: #E8E9EB;
    padding: 4% 2%;
  }
  .right-info{
    text-align: left;
    flex:1;
    width:48%;
    background-repeat: no-repeat;
    background-size: contain;
  }

  .swiper-slide {
    display: flex;
  }
  .star-ratings-sprite {
    background: url("../assets/star-rating-sprite.png") repeat-x;
    font-size: 0;
    height: 21px;
    line-height: 0;
    overflow: hidden;
    text-indent: -999em;
    width: 110px;
    margin: 0 auto;
    margin-left: 1%;
  }
  .star-ratings-sprite-rating {
    background: url("../assets/star-rating-sprite.png") repeat-x;
    background-position: 0 100%;
    float: left;
    height: 21px;
    display:block;
  }
  .swiper-pagination-bullet-active {
      opacity: 1;
      background: #666;
  }
  html,body {
    height:100%;width:100%;margin:0 3%;padding:0;
  }
  .swiper-container{
    padding-bottom: 30px;
  }


  @media only screen and (max-width: 568px){
    .swiper-slide{
      flex-flow: column;
    }
    .left-info,.right-info{
      width:90%;
      margin: 0 auto;
    }
    .left-info{
      order:2;
      margin-bottom: 7%;
    }
    .right-info{
      order:1;
      text-align: center;
      max-width: 50vw;
      height: 60vw;
      margin-bottom: 2%;
    }
    .Top-Movies-This-Week{
      text-align: center;
    }

    .swiper-container-horizontal > .swiper-pagination-bullets {
      bottom: 5px;
      display: none;
    }
    .swiper-pagination-bullet{
      border: #666 solid 1.9px;
      background: #fff;
    }
    .Top-Movies-This-Week{
      margin-top: 1%;
    }
    .In-Theaters{
      margin-bottom: 1%;
      text-align: center;
    }
    .swiper-container{
      padding-bottom: 10px;
    }
    .view-more{
      top:3%;
    }
    #view-more-link{
      font-size: 1.3rem;
    }
    .star-ratings-sprite{
      margin: 0 auto;
    }
  }
</style>
