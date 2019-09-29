<template>
<div class="rate-content">
  <div class="div-rate">
    <span>{{rate}}</span>
    <div class="">
      <i v-if="rate<1 && rate>0" class="fas fa-star-half-alt"></i>
      <i v-else-if="rate>=1" class="fas fa-star"></i>
      <i v-else class="far fa-star"></i>
    </div>
    <div class="">
      <i v-if="rate<2 && rate>3" class="fas fa-star-half-alt"></i>
      <i v-else-if="rate>=2" class="fas fa-star"></i>
      <i v-else class="far fa-star"></i>
    </div>
    <div class="">
      <i v-if="rate<3 && rate>2" class="fas fa-star-half-alt"></i>
      <i v-else-if="rate>=3" class="fas fa-star"></i>
      <i v-else class="far fa-star"></i>
    </div>
    <div class="">
      <i v-if="rate<4 && rate>3" class="fas fa-star-half-alt"></i>
      <i v-else-if="rate>=4" class="fas fa-star"></i>
      <i v-else class="far fa-star"></i>
    </div>
    <div class="">
      <i v-if="rate<5 && rate>4" class="fas fa-star-half-alt"></i>
      <i v-else-if="rate==5" class="fas fa-star"></i>
      <i v-else class="far fa-star"></i>
    </div>

  </div>
</div>
</template>

<script>
export default {
  name: 'Rate',
  props: ['rate'],
  data(){
    return{

    }
  },
  methods:{

  }
}
</script>

<style>
.rate-content{
  display: flex;
  width: 100%;
  flex-direction: row;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
}
.div-rate{
  flex: 1;
  position: relative;
}
.div-rate span, .div-rate i, .div-rate div{
  font-size: 1.3rem;
  flex: 1;
}
.div-rate div{
  display: contents;
}
.div-rate span{
  font-weight: bolder;
  margin-right: 12px;
}
.div-rate i{
  color: #FDCB13
}
</style>
