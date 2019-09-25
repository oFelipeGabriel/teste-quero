<template>
<div class="footer">
  <div class="secundario">
    <div class="group">
      <i class="fab fa-whatsapp"></i>
      <div>
        <span>0800 123 222</span>
        <span>Seg - Sex 8h-22h</span>
      </div>
    </div>
    <div class="group">
      <i class="far fa-comments"></i>
      <div>
        <span>Chat ao vivo</span>
        <span>Seg - Sex 8h-22h</span>
      </div>
    </div>
    <div class="group">
      <i class="far fa-envelope"></i>
      <div>
        <span>Mande um e-mail</span>
        <span>Seg - Sex 8h-22h</span>
      </div>
    </div>
    <div class="group">
      <i class="fas fa-info-circle"></i>
      <div>
        <span>Central de ajuda</span>
        <span>Seg - Sex 8h-22h</span>
      </div>
    </div>

  </div>
  <div class="principal">
    <span>Feito com <i class="far fa-heart"></i> pela Quero Educação</span>
  </div>

</div>
</template>


<script>


</script>

<style>
.principal{
  background-color: #18ACC4;
  height: 100px;
}
.principal span{
  display: inline-flex;
  line-height: 100px;
}
.principal i{
  line-height: 100px;
  margin-left: 3px;
  margin-right: 3px;
}
.secundario{
  background-color: #007A8D;
  height: 70px;
  align-items: center;
  padding: 1.2rem 4rem;
}
.secundario i{
  font-size: 1.8rem;
  float: left;
  margin-right: 5px;
  line-height: 35px;
}
.group{
  display: inline-flex;
  width: 20%;
  padding: 15px;
}
.secundario div div{
  width: 100%;
}
.secundario div div span{
  display: grid;
  text-align: left;
}
.footer{
  color: #FBFBFB;
  font-weight: 600
}
</style>
