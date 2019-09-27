<template>
  <div class="back-modal">
    <span class="fechar" @click="fechaModal"><i class="fas fa-times"></i></span>
    <div class="modal">
      <h2 class="texto">Adicionar bolsa</h2>
      <span class="texto">Filtre e adicione as bolsas de seu interesse.</span>
      <div class="card cidade">
        <h4 class="texto">SELECIONE SUA CIDADE</h4>
        <select class="select-cidade" name="cidade">
          <option value="" selected disabled>Selecione</option>
          <option v-for="(cidade, index) in cidades" :value="index">{{cidade}}</option>
        </select>
      </div>
      <div class="card curso">
        <h4 class="texto">SELECIONE O CURSO DE SUA PREFERÊNCIA</h4>
        <select class="select-curso" name="curso">
          <option value="" selected disabled>Selecione</option>
          <option v-for="(curso, index) in cursos" :value="index">{{curso}}</option>
        </select>
      </div>
      <div class="card como-estudar">
        <h4 class="texto">COMO VOCÊ QUER ESTUDAR:</h4>
        <div class="group-chek">
          <input type="checkbox" name="" value="" id="presencial">
          <label for="presencial">Presencial</label>
        </div>
        <div class="group-chek">
          <input type="checkbox" name="" value="" id="distancia">
          <label for="distancia">A distância</label>
        </div>
      </div>
        <div class="card pagar">
        <h4 class="texto">ATÉ QUANTO PODE PAGAR?</h4>
        <label for="id_valor">R${{valor}}</label>
        <input
          id="id_valor"
          type="range"
          step="0.01"
          :min="minValue"
          :max="maxValue"
          v-model="valor">
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
              <td class="text-right">Nome da Faculdade <i class="fas fa-chevron-down"></i></td>
            </tr>
            <tr v-for="(bolsa, index) in bolsas">
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
                <span class="curso-nome">{{bolsa['course']['name']}}</span>
                <span class="curso-level">{{bolsa['course']['level']}}</span>
                <span class="desconto">Bolsa de <span class="valor-desconto">{{bolsa['discount_percentage']}}%</span></span>
                <span class="curso-valor">R$ {{bolsa['full_price'].toFixed(2).replace('.', ',')}}/mês</span>
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
      cidades: [],
      cursos: [],
      bolsas: [],
      minValue: null,
      maxValue: null,
      valor: 0,
      bolsasSelecionadas: [],
      classBtnAdd: {
        class: "btnAddInactive",
        isActive: false,
      },
    }
  },
  methods:{
    adicionaBolsas(){
      this.$emit('adicionaBolsas', this.bolsasSelecionadas);
    },
    fechaModal(){
      this.$emit('fechaModal');
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
      })
    },
    addBolsa(item){
      this.bolsas[item].selected = true;
      this.bolsasSelecionadas.push(this.bolsas[item])
    },
    removeItem(item){
      this.bolsas[item].selected = false;
      let indice = this.bolsasSelecionadas.indexOf(item);
      this.bolsasSelecionadas.splice(indice, 1);
    }

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

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.back-modal{
  position: absolute;
  top: 0;
  bottom: 0;
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
  padding: 10px 5%;
  overflow: auto;
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
.div-tabela{
  display: flex;
}
table{
  width: 90%;
  flex: 1;
  border-spacing: 0px;
}
table.no-spacing {
  border-spacing:0;
  border-collapse: collapse;
}
table td{
  position: relative;
  height: 2.5rem;
  border-bottom: 2px solid grey;
  padding: .8rem 10px;;
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
.checkbox{
  width: 5px;
  height: 20px;
  border-radius: 6px;
  position: absolute;
  top: 1.3rem;
  padding: 2px 10px;
}
.checkbox-select{
  cursor: pointer;
  border: 1px solid #1F2D30;
  background-color: #FBFBFB
}
.checkbox-selected{
  cursor: pointer;
  color: #FBFBFB;
  background-color: #007A8D;
  border: 1px solid #FBFBFB;
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
</style>
