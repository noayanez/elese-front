<template>
  <div v-if="oculto">
    <div class=" Footer">
      <div class="uk-grid uk-child-width-1-3">
        <div>
          <small>{{title}}</small>
        </div>
        <div class="uk-text-center">
          <small>Dev: {{developer}}</small>
        </div>
        <div class="uk-text-right">
          <router-link to="/Admin"><b v-on:click="ocultar">!Soy Admin!</b></router-link>
        </div>
      </div>
    </div>
  </div>
  <div v-else>
  </div>
</template>

<script>
  export default{
    data(){
      return{
        title : 'Copyright 2019',
        developer : 'NoaYanez',
        oculto : true
      }
    },
    methods:{
      ocultar: function(){
        this.oculto = false;
      }
    }
  }
</script>

<style scoped>
  .Footer{
    background: black;
    padding: 10px;
    height: 25px;
    margin: 0px;
  }

  small{
    color: #fff;
    text-align: center;
  }
</style>

