<template>
<div class="footer">
  <div class="secundario">
    <div class="group-contato">
      <div>
      <i class="fab fa-whatsapp"></i>
      <div>
        <span>0800 123 222</span>
        <span>Seg - Sex 8h-22h</span>
      </div>
    </div>
    </div>
    <div class="groups">
    <div class="group">
      <div class="group-icon">
        <i class="far fa-comments"></i>
      </div>

      <div>
        <span class="mobile">Chat</span>
        <span class="full">Chat ao vivo</span>
        <span class="full">Seg - Sex 8h-22h</span>
      </div>
    </div>
    <div class="group">
      <div class="group-icon">
        <i class="far fa-envelope"></i>
      </div>

      <div>
        <span class="mobile">E-mail</span>
        <span class="full">Mande um e-mail</span>
        <span class="full">Seg - Sex 8h-22h</span>
      </div>
    </div>
    <div class="group">
      <div class="group-icon">
        <i class="fas fa-info-circle"></i>
      </div>

      <div>
        <span class="mobile">Ajuda</span>
        <span class="full">Central de ajuda</span>
        <span class="full">Seg - Sex 8h-22h</span>
      </div>
    </div><!--
    <div class="group">
      <div class="group-icon">
      <i class="far fa-envelope"></i>
      <div>
        <span class="mobile">E-mail</span>
        <span class="full">Mande um e-mail</span>
        <span class="full">Seg - Sex 8h-22h</span>
      </div>
    </div>
    </div>
    <div class="group">
      <div class="">
        <i class="fas fa-info-circle"></i>
        <div>
          <span class="mobile">Ajuda</span>
          <span class="full">Central de ajuda</span>
          <span class="full">Seg - Sex 8h-22h</span>
        </div>
      </div>

    </div> -->
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
  padding-top: .6rem;
  /*height: 70px;*/
  align-items: center;
  /*padding: 1.2rem 4rem;*/
}
.full{
  display: none !important;
}
.secundario i{
  font-size: 1.8rem;
  float: left;
  margin-right: 5px;
  line-height: 35px;
  width: 100%;
}
.group{
  /*display: inline-flex;*/
  display: table;;
  width: 33%;
  padding: 15px;
  border: 1px solid #18ACC4;
  margin-top: 1.2rem;
}
.groups{
  width: 100%;
  display: flex;
}
.group-contato{
  display: inline-flex;
  width: 100%;
}
.group-contato div:first-child{
  margin: 0 auto;
  width: 50%;
  max-width: 180px;
  display: flex;
}
.group-contato i{
  margin-right: 15px;
  width: 10%;
}
.group-contato div{
    text-align: left;
}
.group-contato span{
  text-align: left !important;
}
.secundario div div span{
  display: grid;
  text-align: center;
  width: 100%;
}
/* .secundario .group:first-child{
  width: 100%;
  margin-left: 25%;
  display: flex;
  justify-content: center;
} */
.group div:last-child{
  display: flex;
  text-align: center;
  width: 100%;
}
.group-icon{
  display: table;
  width: 100%;
  text-align: center;
}
.footer{
  color: #FBFBFB;
  font-weight: 600
}
</style>
