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

    </div>
  </div>
</template>

<script>
export default {
  name: 'Index',
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
  min-height:400px;
}
</style>
