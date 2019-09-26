<template>
  <div class="back-modal">
    <span class="fechar" @click="fechaModal"><i class="fas fa-times"></i></span>
    <div class="modal">
      <h2>Adicionar bolsa</h2>
      <span>Filtre e adicione as bolsas de seu interesse.</span>
      <div class="card cidade">
        <h4>SELECIONE SUA CIDADE</h4>
        <select class="select-cidade" name="cidade">

        </select>
      </div>
      <div class="card curso">
        <h4>SELECIONE O CURSO DE SUA PREFERÊNCIA</h4>
        <select class="select-cidade" name="cidade">

        </select>
      </div>
      <div class="card como-estudar">
        <h4>COMO VOCÊ QUER ESTUDAR:</h4>
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
        <h4>ATÉ QUANTO PODE PAGAR?</h4>
        <select class="select-cidade" name="cidade">

        </select>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Modal',
  methods:{
    fechaModal(){
      this.$emit('fechaModal');
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.back-modal{
  position: absolute;
  position: fixed;
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
  width: 100%;
  background-color: white;
  min-height: 90%;
  margin-top: 70px;
  text-align: left;
  padding: 10px 5%;
}

.modal h2{
  font-size: 3rem;
  margin-bottom: .4rem;
}
</style>
