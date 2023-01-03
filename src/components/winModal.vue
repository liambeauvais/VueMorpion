<template>
  <div class="modal-mask">
    <div class="modal-wrapper">
      <div class="modal-container">

        <div class="modal-header">
          <slot name="header">
            La victoire revient à {{winner}}!!
          </slot>
        </div>

        <div class="modal-body">
          <slot name="body">
            <p>Points de {{players.player_1.name}} : {{players.player_1.points}}</p>
            <p>Points de {{players.player_2.name}} : {{players.player_2.points}}</p>
          </slot>
        </div>

        <div class="modal-footer">
          <slot name="footer">
            Voulez-vous rejouer?
            <button class="modal-default-button" @click="$emit('close')">
              Rejouer
            </button>
          </slot>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "winModal",
  props:{
    winner: String,
    players: Object
  }
}
</script>

<style scoped>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: table;
  transition: opacity 0.3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 300px;
  margin: auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
  font-family: Helvetica, Arial, sans-serif;
}

.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}

.modal-body {
  margin: 20px 0;
}

.modal-footer{
  height: 30px;
}


.modal-default-button {
  float: right;
}

button{
  border-radius: 16px;
  background-color: darkblue;
  color: white;
  height: 30px;
  width: 80px;
  padding: 3px;
  text-align: center;
}
</style>