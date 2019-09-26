<template>
  <div class="wrapper">
    <div class="navbar">
      <ul>
        <li>Minha Conta</li>
        <li>Pré-matrículas</li>
        <li>Bolsas Favoritas</li>
      </ul>
    </div>
    <div class="bolsas-favoritas">
      <h2>Bolsas favoritas</h2>
      <p>Adicione bolsas de cursos e faculdades do seu interesse e receba atualizações com as melhores ofertas disponíveis.</p>
      <div class="center">
        <div class="lista">
      <ul class="lista-semestre">
        <li class="active">Todos os semestres</li>
        <li>2º semestre de 2019</li>
        <li>1º semestres de 2020</li>
      </ul>
    </div>
    </div>
      <div class="btn-adicionar" @click="mostraModal">
        <span class="btn-plus"><i class="fas fa-plus-circle"></i></span>
        <h4>Adicionar bolsa</h4>
        <span>Clique para adicionar bolsas de cursos do seu interesse</span>
      </div>
    </div>
    <transition name="slide-fade">
    <Modal v-show="modal==true" @fechaModal="fechaModal"/>
  </transition>
  </div>
</template>

<script>
import Modal from './Modal.vue'
export default {
  name: 'Index',
  components: {
    Modal,
  },
  data(){
    return{
      modal: false,
    }
  },
  methods:{
    mostraModal(){
      this.modal = true;
    },
    fechaModal(){
      this.modal = false;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.navbar{
  background-color: #007A8D;
  height: 50px;
}
.navbar ul{
  display: inline-flex;
  list-style: none;
  text-align: left;
  width: 100%;
  color: #FBFBFB;
}
.navbar li{
  margin: 0 10px;
  cursor: pointer;
  font-weight: 600;
}
.bolsas-favoritas{
  min-height:400px
}
.bolsas-favoritas h2, .bolsas-favoritas p{
  text-align: left;
  padding-left: 5%;
}
.bolsas-favoritas h2{
  font-size: 3rem;
  margin-bottom: .4rem;
}
.bolsas-favoritas p{
  font-size: 1.25rem;
}
.center{
  width: 100%;
  align-items: center;
}
.lista{
  display: grid;
}
.lista-semestre{
  list-style: none;
  width: 90%;
  border-radius: 7px;
  padding-left: 5%;
}
.lista-semestre li{
  border: 1px solid #007A8D;
  padding: 1.2rem 0;
  font-size: 1.4rem;
  color: #007A8D;
  cursor: pointer;
  font-weight: 600;
}
.lista-semestre li:first-child{
  border-radius: 7px 7px 0 0;
}
.lista-semestre li:last-child{
  border-radius: 0 0 7px 7px;
}
.lista-semestre .active{
  background-color: #007A8D;
  color: #FBFBFB;
}
.btn-adicionar{
  box-shadow: 3px 3px 5px rgba(31, 45, 48, .22), -3px -3px 5px rgba(31, 45, 48, .22);
  margin: 1.5rem 5%;
  padding: 1.5rem 0;
  text-align: center;
  cursor: pointer;
}
.btn-adicionar i{
  color: #007A8D;
  font-size: 5rem;
}
.btn-adicionar h4{
  font-size: 1.6rem;
  margin-bottom: 5px;
}
.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to{
  transform: translateY(-40px);
  opacity: 0;
}
</style>
