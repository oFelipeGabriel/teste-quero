<template>
  <div class="back-modal">
    <span class="fechar" @click="fechaModal"><i class="fas fa-times"></i></span>
    <div class="modal">
      <h2 class="texto">Adicionar bolsa</h2>
      <span class="texto">Filtre e adicione as bolsas de seu interesse.</span>
      <div class="group-modal">

        <div class="card cidade">
          <h4 class="texto">SELECIONE SUA CIDADE</h4>
          <select class="select-cidade" name="cidade" @change="changeCidade" v-model="filtros.cidade">
            <option value="null" disabled>Selecione</option>
            <option v-for="cidade in cidades" :value="cidade">{{cidade}}</option>
          </select>
        </div>
        <div class="card curso">
          <h4 class="texto">SELECIONE O CURSO DE SUA PREFERÊNCIA</h4>
          <select class="select-curso" name="curso" @change="filtrar" v-model="filtros.curso">
            <option value="null" disabled>Selecione</option>
            <option v-for="(curso) in cursos" :value="curso">{{curso}}</option>
          </select>
        </div>
      </div>

      <div class="card como-estudar">
        <h4 class="texto">COMO VOCÊ QUER ESTUDAR:</h4>
        <div class="div-group-check">
          <div class="group-chek">
            <input type="checkbox" name="" value="" id="presencial" v-model="filtros.comoEstudar.presencial">
            <div class="checkbox-type">
              <span v-if="!filtros.comoEstudar.presencial" class="checkbox checkbox-select" @click="addType('presencial')"></span>
              <span v-else class="checkbox checkbox-selected" @click="addType('presencial')">
                <i class="fas fa-check"></i>
              </span>
            </div>
            <label for="presencial">Presencial</label>
          </div>
          <div>
            <input type="checkbox" name="" value="" id="distancia" v-model="filtros.comoEstudar.distancia">
            <div class="checkbox-type">
              <span v-if="!filtros.comoEstudar.distancia"  class="checkbox checkbox-select" @click="addType('distancia')"></span>
              <span v-else class="checkbox checkbox-selected" @click="addType('distancia')">
                <i class="fas fa-check"></i>
              </span>
            </div>

            <label for="distancia">A distância</label>
          </div>
        </div>

      </div>
        <div class="card pagar">
        <h4 class="texto">ATÉ QUANTO PODE PAGAR?</h4>
        <label for="id_valor">R$ {{parseFloat(filtros.valor).toFixed(2).replace('.', ',')}}</label>
        <input
          id="id_valor"
          type="range"
          step="0.01"
          :min="minValue"
          :max="maxValue"
          v-model="filtros.valor"
          @change="filtrar">
        </div>
        <!-- tabela de cursos -->
        <div class="div-tabela">
        <table>
          <thead>
            <tr>
              <th>Resultado: </th>
              <th class="text-right">Ordenar por </th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td></td>
              <td class="text-right nome-faculdade-filter" @click="ordenaNome">Nome da Faculdade <i class="fas fa-chevron-down"></i></td>
            </tr>
            <tr v-for="(bolsa, index) in bolsasFilter">
              <td>
                <!-- <input type="checkbox" name="" :value="index"> -->
                <div class="div-check">
                    <span v-if="!bolsa.selected" class="checkbox checkbox-select" @click="addBolsa(index)"></span>
                    <span v-else class="checkbox checkbox-selected" @click="removeItem(index)">
                      <i class="fas fa-check"></i>
                    </span>
                </div>
                <div class="div-imagem">
                    <img :src="require(`@/assets/${bolsa.logo}.png`)" alt="">
                </div>
              </td>
              <td>
                <div class="info-curso">
                  <span class="curso-nome">{{bolsa['course']['name']}}</span>
                  <span class="curso-level">{{bolsa['course']['level']}}</span>
                </div>
                <div class="info-valor">
                  <span class="desconto">Bolsa de <span class="valor-desconto">{{bolsa['discount_percentage']}}%</span></span>
                  <span class="curso-valor">R$ {{bolsa['price_with_discount'].toFixed(2).replace('.', ',')}}/mês</span>
                </div>

              </td>
            </tr>
          </tbody>
        </table>
      </div>
        <div class="btns">
          <button
            type="button"
            name="button"
            id="btn_cancelar"
            @click="fechaModal">Cancelar</button>
          <button
            type="button"
            name="button"
            id="btn_adicionar"
            :class="classBtnAdd.class"
            @click="adicionaBolsas">Adicionar bolsa(s)</button>
        </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'Modal',
  props: ['enabled'],
  data(){
    return{
      filtros:{
        cidade: null,
        curso: null,
        comoEstudar: {
          presencial: true,
          distancia: true,
        },
        valor: 0,
      },
      cidades: [],
      cursos: [],
      bolsas: [],
      bolsasFilter: [],
      minValue: null,
      maxValue: null,
      bolsasSelecionadas: [],
      classBtnAdd: {
        class: "btnAddInactive",
        isActive: false,
      },
    }
  },
  methods:{
    adicionaBolsas(){
      if(this.bolsasSelecionadas.length>0){
          this.$emit('adicionaBolsas', this.bolsasSelecionadas);
      }
    },
    addType(type){
      this.filtros.comoEstudar[type] = !this.filtros.comoEstudar[type];
      this.filtrar();
    },
    fechaModal(){
      this.$emit('fechaModal');
    },
    changeCidade(){
      this.filtros.curso = null;
      this.filtrar();
    },
    filtraCidade(){
      if(this.filtros.cidade!=null){
        let app = this;
        this.bolsasFilter = this.bolsasFilter.filter(function(item){
          if(item['campus']['city'] == app.filtros.cidade){
            return item
          }
        })
        this.cursos = [];
        for(let x=0;x<app.bolsasFilter.length;x++){
          let curso = app.bolsasFilter[x]['course']['name']
          if(!app.cursos.includes(curso)){
            app.cursos.push(curso)
          }
        }
      }
    },
    filtraCursos(){
      let c = this.bolsasFilter;
      let app = this;
      if(this.filtros.curso!=null){
        this.bolsasFilter = c.filter(function(item){
          return item['course']['name'] == app.filtros.curso;
        })
      }
    },
    filtraTipo(){
      let app = this;
      this.bolsasFilter = this.bolsas.filter(function(item){
        if(
          item['course']['kind'] == "Presencial" && app.filtros.comoEstudar.presencial == true
        ){
          return item
        }else if(
          item['course']['kind'] == "EaD" && app.filtros.comoEstudar.distancia == true
        ){
          return item
        }
      })
    },
    filtraValor(){
      let app = this;
      if(this.filtros.valor>0){
        this.bolsasFilter = this.bolsasFilter.filter(function(item){
          return item['price_with_discount'] <= app.filtros.valor;
        })
      }
    },
    filtrar(){
      this.filtraTipo();
      this.filtraCidade();
      this.filtraCursos();
      this.filtraValor();
    },
    getData(){
      let app = this;
      axios.get('https://testapi.io/api/redealumni/scholarships').then(function(res){
        let data = res.data;
        for(let x in data){
          let universidade = data[x]['university']['name'].toLowerCase().replace(' ', '-').replace('á', 'a');
          data[x].logo = universidade;
          data[x].selected = false;
          app.bolsas.push(data[x])
          let cidade = data[x]['campus']['city']
          if(!app.cidades.includes(cidade)){
            app.cidades.push(cidade)
          }
          let curso = data[x]['course']['name'];
          if(!app.cursos.includes(curso)){
            app.cursos.push(curso)
          }
          app.minValue = (app.minValue==null)?data[x]['price_with_discount']:app.minValue;
          app.maxValue = (app.maxValue==null)?data[x]['price_with_discount']:app.maxValue;
          if(app.minValue>data[x]['price_with_discount']){
            app.minValue = parseFloat(data[x]['price_with_discount']).toFixed(2)
          }
          if(app.maxValue<data[x]['price_with_discount']){
            app.maxValue = parseFloat(data[x]['price_with_discount']).toFixed(2)
          }
        }
        app.bolsasFilter = app.bolsas;
      })
    },
    addBolsa(item){
      this.bolsasFilter[item].selected = true;
      this.bolsasSelecionadas.push(this.bolsasFilter[item])
    },
    removeItem(item){
      this.bolsas[item].selected = false;
      let indice = this.bolsasSelecionadas.indexOf(item);
      this.bolsasSelecionadas.splice(indice, 1);
    },
    ordenaNome(){
      this.bolsasFilter.sort(function(a, b){
          if(a['university']['name'] > b['university']['name']){
            return 1
          }else{
            return -1
          }
        });
    },

  },
  watch:{
    enabled(){
      if(this.enabled){
        this.getData();
      }
    },
    bolsasSelecionadas(){
      if(this.bolsasSelecionadas.length>0){
        this.classBtnAdd.class = "btnAddActive",
        this.classBtnAdd.isActive = true;
      }else{
        this.classBtnAdd.class = "btnAddInactive";
        this.classBtnAdd.isActive = false;
      }
    }
  }
}
</script>

<style scoped>
.back-modal{
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  background-color: rgba(31, 45, 48, .22);
  width: 100%;
  height: 100%;
}
.fechar{
  position: absolute;
  right: 10px;
  top: 10px;
  font-size: 4rem;
  color: white;
  cursor: pointer;
}
.modal{
  background-color: white;
  min-height: 90%;
  margin-top: 70px;
  text-align: left;
  padding: 10px 1rem;
  overflow: auto;
  z-index: 999;
}
.group-modal{
  display: grid;
}
.text-right{
  text-align: right;
}
.modal h2{
  font-size: 3rem;
  margin-bottom: .4rem;
}
select{
  color: #1F2D30;
  height: 2.2rem;
  font-size: 1.3rem;
  border-radius: 5px;
  background-color: white;
  cursor: pointer;
}
select option:checked, select option:focus{
  background-color: #007A8D;
  color: #FBFBFB;
  cursor: pointer
}
.card h4{
  margin-bottom: 2px;
}
.pagar label{
  width: 100%;
  display: block;
  margin-bottom: .5rem;
}
.div-group-check{
  display: flex;
  margin-top: 1rem;
}
.div-group-check label{
  margin: 0 0.8rem;
  font-size: 1.3rem;
}
.group-check{
  margin-right: .5rem;
}
.checkbox-type{
  position: relative;
  float: left;
  flex: 1;
}
.checkbox-type .checkbox{
  position: absolute;
  padding: 2px 10px;
  width: 2px;
  height: 16px;
  border-radius: 6px;
}

.div-tabela{
  display: flex;
}
table{
  width: 90%;
  flex: 1;
  border-spacing: 0px;
  margin-top: 1rem;
}
table.no-spacing {
  border-spacing:0;
  border-collapse: collapse;
}
table td{
  position: relative;
  height: 2.5rem;
  border-bottom: 2px solid #C9CDCE;
  padding: .8rem .5rem;
}
table td:first-child, .nome-faculdade-filter{
  padding: 0
}
.nome-faculdade-filter{
  color: #007A8D;
  font-weight: 600;
  cursor: pointer;
}
.div-imagem{
  text-align: center;
}
img{
  flex: 1;
  max-width: 19vh;
  max-height: 70px;
  margin-left: 10vw;
}
.div-check{
  position: absolute;
  top: 30%;
}
table .checkbox{
  width: 5px;
  height: 20px;
  border-radius: 6px;
}
table .checkbox{
  position: absolute;
  top: 1.3rem;
  padding: 2px 10px;
}
.checkbox-select{
  cursor: pointer;
  border: 1px solid #1F2D30;
  background-color: #FBFBFB;
}
table .checkbox-selected, .div-group-check .checkbox-selected{
  cursor: pointer;
  color: #FBFBFB;
  background-color: #007A8D;
  border: 1px solid #FBFBFB;
  width: auto;
}
table .checkbox-selected{
  padding: 4px 5px 2px;
}
.checkbox-type .checkbox-selected{
  padding: 3px 4px;
}
.curso-nome{
  color: #007A8D;
  display: block;
  font-size: 1.3rem;
  font-weight: bold;
  margin-bottom: .7rem;
}
.curso-level{
  margin-bottom: 1.1rem;
}
.curso-level, .desconto{
  display: block;
}
.desconto{
  margin-bottom: .4rem;
  font-size: 1.2rem;
}
.info-valor{
  text-align: left;
}
.valor-desconto, .curso-valor{
  color: #0FA866;
  font-weight: 800;
  font-size: 1.2rem;
}
.btns{
  display: flex;
  margin-top: 10px;
}
.btns button{
  margin: 6px;
  border-radius: 6px;
  padding: 13px 5px;
  font-weight: 700;
  font-size: 1.2rem;
  cursor: pointer;
}
#btn_cancelar{
  width: 41%;
  background-color: #FBFBFB;
  border: 1px solid #18ACC4;
  color: #18ACC4;
}
.btnAddActive{
  width: 58%;
  background-color: #FDCB13;
  border: 1px solid #DE9E1F;
  color: #1F2D30;
}
.btnAddInactive{
  width: 58%;
  background-color: #C9CDCE;
  border: 1px solid #7E8283;
  color: #7E8283;
}
input[type=range] {
  -webkit-appearance: none;
  margin: 18px 0;
  width: 100%;
}
input[type=range]:focus {
  outline: none;
}
input[type=range]::-webkit-slider-runnable-track {
  width: 40%;
  height: 8.4px;
  cursor: pointer;
  animate: 0.2s;
  background: #18ACC4;
  border-radius: 5px;
}
input[type=range]::-webkit-slider-thumb {
  border: 4px solid #18ACC4;
  height: 35px;
  width: 34px;
  border-radius: 35px;
  background: #ffffff;
  cursor: pointer;
  -webkit-appearance: none;
  margin-top: -14px;
}
input[type=range]:focus::-webkit-slider-runnable-track {
  background: #18ACC4;
}
input[type=range]::-moz-range-track {
  width: 40%;
  height: 8.4px;
  cursor: pointer;
  animate: 0.2s;
  background: #3071a9;
  border-radius: 1.3px;
  border: 0.2px solid #010101;
}
input[type=range]::-moz-range-thumb {
  border: 1px solid #000000;
  height: 25px;
  width: 25px;
  border-radius: 25px;
  background: #ffffff;
  cursor: pointer;
}
input[type=range]::-ms-fill-lower {
  background: #18ACC4;
  border: 0.2px solid #010101;
  border-radius: 2.6px;
}
input[type=range]::-ms-fill-upper {
  background: #18ACC4;
  border: 0.2px solid #010101;
  border-radius: 2.6px;
}
input[type=range]::-ms-thumb {
  border: 1px solid #18ACC4;
  height: 36px;
  width: 16px;
  border-radius: 3px;
  background: #ffffff;
  cursor: pointer;
}
input[type=range]:focus::-ms-fill-lower {
  background: #18ACC4;
}
input[type=range]:focus::-ms-fill-upper {
  background: #18ACC4;
}

@media only screen and (min-width: 900px) {
  .modal{width: 60%; margin-left: 22%;height: 80%; overflow: auto;}
  .fechar{ right: 16%;}
  .group-modal{ display: flex}
  .card{padding: 0 5px;}
  .card h4{font-size: .6rem}
  select{width: 100%;}
  .div-group-check{margin-bottom: 1.5rem;}
  .checkbox-type .checkbox-selected{padding: 3px 2px 1px 3px;}
  .checkbox-type .checkbox.select{padding: 1.5px 5px; font-size: 12px;}
  .div-group-check label{margin: 0 0.8rem 0 .5rem;font-size: .9rem;}
  table .checkbox{ top: .3rem;}
  .div-imagem{ margin-right: -26px}
  .modal img{ margin-left: 2vw;}
  .info-curso{float: left;}
  .info-valor{text-align: right;}
  .curso-nome{font-size: .9rem;}
  .curso-level{font-size: .8rem;}
  .curso-valor{ font-size: 1rem;}
  .desconto{font-size: .9rem; margin-left: 4vw}
}
</style>
