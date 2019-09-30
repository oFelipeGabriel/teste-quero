<template>
  <div class="wrapper">
    <div class="navbar">
      <ul class="navbar-links">
        <li>Minha Conta</li>
        <li>Pré-matrículas</li>
        <li class="active">Bolsas Favoritas</li>
      </ul>
      <ul class="nav-menu">
        <li>Menu <i class="fas fa-chevron-down"></i></li>
      </ul>
    </div>
    <div class="breadcrumbs">
      <ul>
        <li class="link">Home</li>
        <li class="separator"> / </li>
        <li class="link">Minha conta</li>
        <li class="separator"> / </li>
        <li class="active">Bolsas favoritas</li>
      </ul>
    </div>
    <div ref="bolsasFav" id="bolsas-fav" class="bolsas-favoritas">
      <h2>Bolsas favoritas</h2>
      <p>Adicione bolsas de cursos e faculdades do seu interesse e receba atualizações com as melhores ofertas disponíveis.</p>
      <div class="center">
        <div class="lista">
      <ul class="lista-semestre">
        <li :class="activeItem['0']" @click="filtraSemestre('all', '0')">Todos os semestres</li>
        <li :class="activeItem['1']" @click="filtraSemestre('2019.2', '1')">2º semestre de 2019</li>
        <li :class="activeItem['2']" @click="filtraSemestre('2020.1', '2')">1º semestres de 2020</li>
      </ul>
    </div>
    </div>
  </div>

  <div class="cards">
      <div class="btn-adicionar" @click="mostraModal">
        <span class="btn-plus"><i class="fas fa-plus-circle"></i></span>
        <h4>Adicionar bolsa</h4>
        <span>Clique para adicionar bolsas de cursos do seu interesse</span>
      </div>
        <transition name="slide-fade">
    <Modal v-show="modal==true"
      :enabled="modal"
      @fechaModal="fechaModal"
      @adicionaBolsas="adicionaBolsas"/>
  </transition>
    <div class="card-oferta" v-for="oferta in bolsasFilter">
      <div class="img-oferta">
          <img :src="require(`@/assets/${oferta.logo}.png`)" alt="">
      </div>

      <span class="oferta-universidade">{{oferta['university']['name']}}</span>
      <span class="oferta-curso">{{oferta['course']['name']}}</span>
      <Rate class="oferta-rate" :rate="oferta['university']['score']" />
      <span class="oferta-periodo">{{oferta['course']['kind']}} - {{oferta['course']['shift']}}</span>
      <span class="oferta-inicio">Inicio das aulas em: {{oferta['start_date']}}</span>
      <span class="oferta-mensalidade-label">Mensalidade com a Quero Bolsa: </span>
      <span class="oferta-mensalidade-full-price">R$ {{String(oferta['full_price']).replace('.', ',')}}</span>
      <span class="oferta-mensalidade-price"><span class="valor-oferta">R$ {{String(oferta['price_with_discount']).replace('.', ',')}}</span> /mês</span>
      <div class="oferta-btns">
        <button type="button" name="button" class="btn-excluir">Excluir</button>
        <button type="button" name="button" class="btn-ver">Ver oferta</button>
      </div>

    </div>
</div>
  </div>
</template>

<script>
import Modal from './Modal.vue'
import Rate from './Rate.vue'
export default {
  name: 'Index',
  components: {
    Modal,
    Rate
  },
  data(){
    return{
      modal: false,
      bolsasSelecionadas: [],
      bolsasFilter: [],
      activeItem:{
        '0': "active",
        '1': "",
        '2': ""
      }
    }
  },
  methods:{
    adicionaBolsas(bolsas){
      this.bolsasSelecionadas = bolsas;
      this.bolsasFilter = bolsas;
      this.modal = false;
      this.mudaClasse('0');
      this.$refs.bolsasFav.scrollIntoView({behavior: "smooth", block: "start", inline: "nearest"});
    },
    mostraModal(){
      this.modal = true;
    },
    fechaModal(){
      this.modal = false;
      this.$refs.bolsasFav.scrollIntoView({behavior: "smooth", block: "start", inline: "nearest"});
    },
    filtraSemestre(sem, item){
      if(sem=='all'){
        this.bolsasFilter = this.bolsasSelecionadas
      }else{
        this.bolsasFilter = this.bolsasSelecionadas.filter(function(item){
          return item['enrollment_semester'] == sem
        })
      };
      this.mudaClasse(item)
    },
    mudaClasse(i){
      for (let x in this.activeItem){
        this.activeItem[x] = ""
      };
      this.activeItem[i] = "active"
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.navbar{
  background-color: #18ACC4;
  height: 50px;
  display: flex;
}
.navbar-links{
  height: 100%;
  margin-top: 0 !important;
  line-height: 50px;
}
.navbar-links .active{
  background-color: #007A8D;
}
.navbar ul, .nav-menu{
  display: inline-flex;
  list-style: none;
  text-align: left;
  color: #FBFBFB;
  padding-left: 0.5rem;
  width: 50%;
}
.navbar li{
  margin: 0 10px;
  cursor: pointer;
  font-weight: 600;
  display: none;
}
.navbar li:first-child{
  display: block;
}
.nav-menu{
  position: relative;
}
.nav-menu li:first-child{
  position: absolute;
  right: 0.5rem;
}
.breadcrumbs{
  display: none;
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
.card-oferta{
  margin: 1.5rem 5%;
  box-shadow: 3px 3px 5px rgba(31, 45, 48, .22), -3px -3px 5px rgba(31, 45, 48, .22);
  display: flex;
  flex-direction: column;
  padding: 2vh;
}
.img-oferta{
  width: 100%;
  display: flex;
  justify-content: center;
}
.card-oferta img{
  max-width: 19vh;
  max-height: 70px;
}
.oferta-universidade, .oferta-periodo, .oferta-rate, .oferta-mensalidade-label{
  font-weight: 800;
  margin-top: .6rem;
}
.oferta-curso{
  font-weight: 800;
  color: #007A8D;
}
.oferta-rate, .oferta-inicio{
  padding-bottom: .6rem;
  border-bottom: 2px solid #C9CDCE;
  margin-bottom: .6rem;
}
.oferta-inicio{
  margin-top: .6rem;
}
.oferta-mensalidade-full-price{
  text-decoration: line-through;
  display: block;
  margin-top: .6rem;
}
.valor-oferta{
  color: #0FA866;
  font-size: 1.6rem;
  font-weight: bolder;
}
.oferta-btns button{
  margin: 25px 5px 5px 5px;
  border-radius: 6px;
  padding: 13px 5px;
  font-weight: 700;
  font-size: 1.2rem;
  cursor: pointer;
}
.btn-excluir{
  width: 41%;
  background-color: #FBFBFB;
  border: 1px solid #18ACC4;
  color: #18ACC4;
}
.btn-ver{
  width: 51%;
  background-color: #FDCB13;
  border: 1px solid #DE9E1F;
  color: #1F2D30;
}

/* desktop */
@media only screen and (min-width: 900px) {
  .nav-menu{ display: none !important;}
  .navbar li { display: block; padding: 0 15px; font-size: .8rem;font-weight: 700}
  .breadcrumbs{ position: relative; }
  .breadcrumbs ul{ position: absolute; left: 0; display: inline-flex; list-style: none; width: 40%; font-size: .8rem; }
  .breadcrumbs .separator{ margin: 0 10px; color: #2c3e50;}
  .breadcrumbs .link{ color: #18acc4; font-weight: 700; cursor: pointer; }
  .breadcrumbs .active{ color: #2c3e50; }
  .bolsas-favoritas{ min-height: 0 }
  .lista-semestre{ display: flex; justify-content: flex-end;}
  .lista-semestre li:first-child{ border-radius: 6px 0 0 6px}
  .lista-semestre li:last-child{ border-radius: 0 6px 6px 0}
  .lista-semestre li{ padding: .4rem 1.2rem; font-size: .8rem;}
  .btn-adicionar{ width: 253px; height: 165px; padding: 125px 0; float: left; margin: 10px; }
  .btn-adicionar h4{font-size: 1.2rem;}
  .btn-adicionar span{ font-size: .8rem; }
  .cards{ display: table; padding: 0 3%;}
  .cards .card-oferta{float:left;margin: 10px; height: 390px;}
  .oferta-mensalidade-label, .oferta-inicio{font-size: .9rem;}
  .oferta-btns button{font-size: .9rem; margin: 25px 3px 5px 5px;}
}
</style>
