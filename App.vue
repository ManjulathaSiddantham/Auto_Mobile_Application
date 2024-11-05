<template>
    <div id="app">
     
     <AutomobilesCrud />
    </div>
  </template>
  
  <script>
  import AutomobilesCrud from './components/AutomobilesCrud.vue';
  export default {
    name: 'App',
    components:{
      AutomobilesCrud
    }
  }
  </script>