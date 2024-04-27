<template>    
  <div id="app">      
    <Economia />    
  </div>  
</template>    
<script>  
  import Economia from './components/economia-familiar.vue' 
  export default {    
    name: 'app',    
    components: {      
      Economia    
    }  
  }  
</script>    
<style>  
  #app {    
    font-family: 'Avenir', Helvetica, Arial, sans-serif;    
    -webkit-font-smoothing: antialiased;    
    -moz-osx-font-smoothing: grayscale;    
    text-align: left;    
    color: #2c3e50;    
    margin-top: 60px;  
  }  
</style>