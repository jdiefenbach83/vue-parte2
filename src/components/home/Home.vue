<template>
  <div>
    <h1 class="centralizado" v-text="titulo"></h1>
    
    <input 
      type="search" 
      class="filtro" 
      @input="filtro = $event.target.value" 
      placeholder="filtre por parte do título"
    >
    
    <ul class="lista-fotos">
      <li class="lista-fotos-item" v-for="foto of fotosComFiltro" :key="foto.titulo">        
        <meu-painel :titulo="foto.titulo">
          <imagem-responsiva v-meu-transform:scale.animacao="1.2" :url="foto.url" :titulo="foto.titulo" />
          <meu-botao 
            tipo="button" 
            rotulo="REMOVER" 
            @botaoAtivado="remove(foto)"
            :confirmacao="true"
            estilo="perigo"
          />
        </meu-painel>
      </li>    
    </ul>
  </div>
</template>

<script>
import Painel from '../shared/painel/Painel.vue';
import ImagemResponsiva from '../shared/imagem-responsiva/ImagemResponsiva.vue';
import Botao from '../shared/botao/botao.vue';

export default {
  components: {
    'meu-painel': Painel,
    'imagem-responsiva': ImagemResponsiva,
    'meu-botao': Botao,
  },
  data() {
    return {
      titulo: 'Alurapic',
      fotos: [],
      filtro: '',
    }
  },
  computed: {
    fotosComFiltro () {
      if (this.filtro){
        const esp = new RegExp(this.filtro.trim(), 'i');

        return this.fotos.filter(foto => esp.test(foto.titulo));
      } else {
        return this.fotos;
      } 
    }
  },
  created() {
    this.$http.get('http://localhost:3000/v1/fotos')
      .then(res => res.json())
      .then(fotos => this.fotos = fotos, err => console.log(err));
  },
  methods: {
    remove(foto) {
      alert('Remover foto! ' + foto.titulo);      
    }
  },
}
</script>

<style scoped>
  .centralizado {
    text-align: center;
  }

  .lista-fotos {
    list-style: none;
  }

  .lista-fotos .lista-fotos-item {
    display: inline-block;
  }

  .filtro {
    display: block;
    width: 100%;
  }
</style>
