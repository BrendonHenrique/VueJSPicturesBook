
<template>

  <div class="corpo">
    <h1 class="centralizado">{{titulo}}</h1>

    <input type="search" class="filtro" @input= "filtro = $event.target.value"  placeholder="Filtre pelo titulo">
    <ul class="lista-fotos">
      <li class="lista-fotos-item" v-for="foto of fotosComFiltro" :key="foto.id">


        <meu-painel :titulo="foto.titulo" >
          <imagem-responsiva :url="foto.url" :titlo="foto.titulo"/>
        </meu-painel>

      </li>
    </ul>
  </div>
</template>

<script>

import Painel from './components/shared/painel/Painel.vue'
import imagemResponsiva from './components/shared/imagem-responsiva/imagemResponsiva.vue'


export default {

  components:{
    'meu-painel':Painel,
    'imagem-responsiva':imagemResponsiva
  },

  data(){
    return {
      titulo:'Interative Pictures',
      fotos:[],
      filtro: ''
    }
  },

  computed:{

    fotosComFiltro(){
      if(this.filtro){
        let exp =  new RegExp(this.filtro.trim(), 'i');
        return this.fotos.filter(foto => exp.test(foto.titulo));
      }else{
        return this.fotos;
      }
    },

  },

  methods:{


  },



  created(){

    this.$http.get('http://localhost:3000/v1/fotos')
      .then(res => res.json())
      .then(fotos => this.fotos = fotos, error => console.log(error));
  }

}

</script>

<style>

.corpo{

  font-family: Arial, Helvetica, sans-serif;
  width: 96%;
  margin: 0 auto;
}

.centralizado{

  text-align: center;
}

.lista-fotos{
  list-style: none;
  display: inline-block;

}

.lista-fotos-item {
  display: inline-block;
}


.filtro{
  display:block;
  width: 100%;
}


</style>
