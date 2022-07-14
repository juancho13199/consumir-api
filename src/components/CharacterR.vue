<template>
  
  <div class="column is-12-mobile is-4-desktop is-4-tablet" >
        <div class="card">

          <div class="card-header">
            <img :src="caracter.image" :alt="caracter.name">
          </div>

          <div class="card-content">
            <h3 class="title is-size-4">{{caracter.name}}</h3>
            <button class="button is-success is-rounded is-small">
              Ver más
            </button>
    
          </div>
        </div>
    </div>

</template>

<script>
export default {
//Importamos el caracter del ciclo for de la app principal
props:['caracter']
}
</script>

<style>

</style>