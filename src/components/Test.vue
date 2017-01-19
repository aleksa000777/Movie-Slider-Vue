<template>
  <div class="movie">
    <div class="header">
      <h1 class="In-Theaters">{{title}}</h1>
      <h2 class="Top-Movies-This-Week">{{subTitle}}</h2>
    </div>
    <div class="view-more">
      <a id="view-more-link" href="" target="_blank">View More</a>
    </div>
    <swiper :options="swiperOption">
      <swiper-slide v-for="movie in movies">
        <div class="left-info">
          <span class="One-line-movie-title">{{movie.Title}}</span>
          <p>
            <span class="desc">{{movie.Plot}}</span>
          </p>
          <div class="star-ratings-sprite">
            <span :style="{width: movie.imdbRating*10+'%'}" class="star-ratings-sprite-rating"></span>
          </div>
        </div>
        <div class="right-info" :style="{backgroundImage: 'url(' + movie.Poster + ')'}">
        </div>
      </swiper-slide>
      <div class="swiper-pagination"  slot="pagination"></div>
    </swiper>
  </div>
</template>



<script>
  const axios = require('axios');
  const movieList = [
    ['ROGUE ONE: A STAR WARS STORY','https://www.rottentomatoes.com/m/rogue_one_a_star_wars_story/'],
    ['Sing','https://www.rottentomatoes.com/m/sing_2016'],
    ['passengers','https://www.rottentomatoes.com/m/passengers_2016'],
    ['MOANA','https://www.rottentomatoes.com/m/moana_2016'],
    ['FANTASTIC BEASTS AND WHERE TO FIND THEM','https://www.rottentomatoes.com/m/fantastic_beasts_and_where_to_find_them']
  ]
  export default {
    name: 'movie',
    data(){
      return{
        title: "In Theaters",
        subTitle: "Top Movies This Week",
        movieList,
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
          loop : false,
          onInit(swiper){
            const index = swiper.activeIndex;
            changeLink(index)
          },
          onSlideChangeEnd(swiper){
            const index = swiper.activeIndex;
            changeLink(index)
          },
        },
      }
    },
    created: function(){
        var self= this;
        for(var i=0;i<this.movieList.length;i++){
          var movie = self.movieList[i][0];
          axios.get('http://www.omdbapi.com/?t='+movie+'&y=&plot=short&r=json')
            .then(function(response){
              self.movies.push(response.data)
            })
            .catch(function(error){
              console.log(error);
            });
        }
    }
  }
  function changeLink(index){
    const linkViewMore = document.getElementById('view-more-link');
    linkViewMore.href = movieList[index][1];
  }
</script>



<style scoped>
  .In-Theaters {
    font-family: Roboto;
    font-size: 24px;
    font-weight: 300;
    font-style: normal;
    font-stretch: normal;
    color: #3b3f47;
    text-align: left;
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
    background: url("https://s3-us-west-2.amazonaws.com/s.cdpn.io/2605/star-rating-sprite.png") repeat-x;
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
    background: url("https://s3-us-west-2.amazonaws.com/s.cdpn.io/2605/star-rating-sprite.png") repeat-x;
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
