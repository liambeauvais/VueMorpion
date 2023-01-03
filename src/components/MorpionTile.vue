<template>
  <div class="morpion">
    <span>{{value}}</span>
  </div>
</template>

<script>
export default {
  name: "MorpionTile",
  props: {
    value : String
  },
  data(){
    return {

    }
  }
}
</script>

<style scoped>
  span{
    font-size: 200px;
    font-weight: bolder;
    justify-self: center;
    align-self: center;
  }
  .morpion{
    border: 1px solid black;
  }
  @media(max-width: 650px){
    span{
      font-size: 100px;
    }
  }
</style>