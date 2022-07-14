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
    @showModal="showModal"
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
  
<div class="modal"
  :class="{ 'is-active': modal}"
  v-if="modal">
  <div class="modal-backgorund" @click="modal=false;">
    <div class="modal-card">

      <header class="modal-card-head">
      <p class="modal-card-title">Acerda de: {{currentCaracter.name}}</p>
      </header>

      <div class="modal-card-body">
      <strong><p>Genero:</p></strong>
      <p>{{currentCaracter.gender}}</p>
       <strong><p>Estatus:</p></strong>
      <p>{{currentCaracter.status}}</p>
       <strong><p>Especie:</p></strong>
      <p>{{currentCaracter.species}}</p>
       <strong><p>Tipo:</p></strong>
      <p>{{currentCaracter.type}}</p>
      </div>
      
      <footer class="modal-card-food">
        <button class="button">Cerrar</button>
      </footer>

    </div>
  </div>
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
      modal:false,
      currentCaracter:{}
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
      //Scroll top
      window.scrollTo(0,0);
    },
    searchData(){
      this.page=1
      this.fetch();
    },
    showModal(id){
      //id
      this.fetchOne(id);
      //fetchOne
    },
    async fetchOne(id){
      //llamado HTTP
      let result= await axios.get(`https://rickandmortyapi.com/api/character/${id}/`);
      this.currentCaracter=result.data;
      this.modal= true;

      console.log(this.currentCaracter,"Personaje");

    }
  }
}
</script>

<style>


</style>
