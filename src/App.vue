<template>

    <div class="hero is-white is-gradien is-bold">
    <div class="hero body">
    <h1 class="title">
      <span class="has-text-success">R&M</span>
      <span class="subtitle">Personajes</span>
    </h1>

      <div class="field has-addons is-pulled-right">
      <div class="control">
      <input v-model="search" 
      type="text" class="input is-rounded" 
      v-on:keyup.enter="searchData">
      </div>
      <div class="control">
            <button class="button is-success is-rounded" 
            @click="searchData">
            Buscar
            </button>
            
      </div>
      </div>

          
    </div>
</div>

<br/>

<div class="container">
    <!--Tarjeta -->
  <div class="columns is-desktop is-mobile is-tablet is-multiline is-centered">
    <CharacterR 
    v-for="caracter of characters" 
    :key="caracter.id" 
    :caracter="caracter"/>
  </div>

  <nav class="pagination" role="navegation" aria-label="pagination">
      <a  class="pagination-previous" @click="changuePage(page -1)">Anterior</a>
          <ul class="pagination-list">
            <li>
              <a class="pagination-ling is-current">{{page}}</a>
            </li>
          </ul>
      <a  class="pagination-next"  @click="changuePage(page +1)">Siguiente</a>
  </nav>
</div>
  

  
</template>

<script>
import axios from 'axios';
import CharacterR from './components/CharacterR.vue'
export default {
  name: 'App',
  data(){
    return{
      characters:[],
      page:1,
      pages:1,
      search:'',
    }
  },
  created(){
    this.fetch();
  },
  components: {
    CharacterR,
  },
  methods:{
    fetch(){
      const params={
        page:this.page,
        name:this.search
      }
      let result= axios
      .get("https://rickandmortyapi.com/api/character",{params})
      .then( res=>{
        this.characters= res.data.results;
        console.log(res.data.info);
        this.pages=res.data.info.pages;
         console.log(res.data);
      })
      .catch(err =>{
        console.log(err);
      })
     console.log("Hola mundo");
    },

    changuePage(page){
      this.page=(page <=0 || page > this.pages) ? this.page:page
      this.fetch();
    },
    searchData(){
      this.page=1
      this.fetch();
    }
  }
}
</script>

<style>


</style>
