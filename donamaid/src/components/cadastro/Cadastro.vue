<template>
 

  <div class="centralizado">
    <img src="/static/donie.png"  >
    
    <h1 class="centralizado"></h1>
    <img src="/static/teste.png">
    <h2 class="centralizado">{{ foto.titulo }}</h2>

    <h2 v-if="foto._id" class="centralizado">Alterando</h2>
    <h2 v-else class="centralizado"></h2>
    

      <div class="controle">
        <label for="titulo">Seu e-mail</label>
        <input data-vv-as="título" name="titulo" v-validate data-vv-rules="required|min:3|max:30" id="titulo" autocomplete="off" v-model="foto.titulo">
        <span class="erro" v-show="errors.has('titulo')">{{ errors.first('titulo') }}</span>
      </div>

       <div class="controle">
        <label for="senha">Sua senha</label>
        <input data-vv-as="senha" name="senha" v-validate data-vv-rules="required|min:3|max:30" id="senha" autocomplete="off" v-model="foto.senha">
        <span class="erro" v-show="errors.has('senha')">{{ errors.first('senha') }}</span>
      </div>



      <div class="centralizado">
        <meu-botao rotulo="Entrar" tipo="submit"/>
        <div class="centralizado">
        <router-link :to="{ name: 'home'}"><meu-botao rotulo="Esqueceu sua senha?" tipo=""/></router-link>
        </div>
      </div>

    </form>
  </div>
</template>

<script>

import ImagemResponsiva from '../shared/imagem-responsiva/ImagemResponsiva.vue'
import Botao from '../shared/botao/Botao.vue';
import Foto from '../../domain/foto/Foto';
import FotoService from '../../domain/foto/FotoService';

export default {

  components: {

    'imagem-responsiva': ImagemResponsiva, 
    'meu-botao': Botao
  },

  data() {

      return {

          foto: new Foto(),
          id: this.$route.params.id
      }
  },

  methods: {

      grava() {

         this.$validator
          .validateAll()
          .then(success => {

            if(success) {

              this.service
                .cadastra(this.foto)
                .then(() => {
                  if(this.id) this.$router.push({ name: 'home' });
                  this.foto = new Foto();
                }, err => console.log(err));
            }
          });
      }
  },

  created() {

      this.service = new FotoService(this.$resource);

      if(this.id) {
        this.service
          .busca(this.id)
          .then(foto => this.foto = foto);
      }
  }
}

</script>
<style scoped>

  .centralizado {
    text-align: center;
     color: black;

  }
  .controle {
    font-size: 1.2em;
    margin-bottom: 20px ;
     color: black;

  }
  .controle label {
    display: block;
    font-weight: bold;
   
  }

 .controle label + input, .controle textarea {
    width: 30%;
    font-size: inherit;
    border-radius: 5px
   

  }

  .centralizado {
    text-align: center;
     
  }

  .erro {
    color: red;
  } 


  
</style>