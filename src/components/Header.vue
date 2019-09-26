<template>
<div class="header">
  <div class="como-funciona">
    <label>
      <i class="fas fa-info-circle"></i>
      Como funciona
    </label>
  </div>
  <div class="contato">
    <div class="">
        <i class="fab fa-whatsapp"></i>
    </div>
    <div class="meios-contatos">
      <span>0800 123 222</span>
      <span>Envie mensagem ou ligue</span>
  </div>
  </div>
  <div class="logo">
    <img src="../assets/logo-querobolsa.svg" alt="">
  </div>
  <div class="user">
    <span>Nomo Sobrenome</span>
    <i class="far fa-user-circle"></i>
  </div>
</div>
</template>


<script>

export default{
}
</script>

<style>
.header{
  display: flex;
  height: 50px;
  padding-bottom: 5px;
  color: #18ACC4;
  padding-top: 15px;
  vertical-align: middle;
}
.como-funciona{
  vertical-align: middle;
  border-right: 2px solid rgba(31, 45, 48,0.2);
  position: relative;
  width: 12%;
}
.como-funciona label{
  line-height: 50px;
}
.contato{
  display: flex;
  width: 18%;
}
.contato i{
  color: #25d366;
  font-size: 1.8rem;
  padding-left: 10px;
  padding-right: 10px;
  line-height: 50px;
}
.contato .meios-contatos{
  text-align: left;
  font-weight: 700;
  font-size: .7rem;
  padding-top: 9px;
}
.meios-contatos span{
    display: inline-table;
    line-height: 15px;
}
.logo{
  width: 45%;
}
.logo img{
  height: 100%;
}
.user{
  text-align: right;
  width: 25%;
  line-height: 50px;
}
.user span{
  font-weight: 700;
  font-size: .9rem;
  padding-right: 10px;
}
.user i{
  font-size: 1.5rem;
}
</style>
