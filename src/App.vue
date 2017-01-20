<template>
  <div id="app">
    <test></test>
  </div>
</template>
<script>
import Test from './components/test'

export default {
  name: 'app',
  components: {
    'test': Test
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  position: relative;
  max-width: 700px;
  margin: 0 auto;
  width:92%;
  top: 20%;
  transform: translateY(20%);
  border: 1px solid #E8E9EB;
  padding: 1.5%;
}
.swiper-pagination-bullet-active {
  opacity: 1;
  background: #6b6b6b !important;
}
.swiper-pagination-bullet{
  background: #fff;
  border: 1.8px solid #666;
}
@media only screen and (max-width: 568px){
  #app {
    margin: 4%;
    top: 0;
    transform:none;
  }
}
</style>
