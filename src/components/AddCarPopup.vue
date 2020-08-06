<template>
  <div class="darkBg">
    <div class="popup">
      <div>Adding new car</div>
      <div class="inputWrp">
        <input type="text" placeholder="max weight (kg)" v-model="maxWeight">
      </div>
      <div class="btnWrp">
        <div class="btn cancel" v-on:click="cancel">cancel</div>
        <div class="btn add" v-on:click="addNewCar">add</div>
      </div>
    </div>
  </div>
</template>

<script>
import Car from '@/scripts/Car.js';

export default {
  data: ()=>(
    {
      maxWeight: null
    }
  ),
  
  methods: {
    addNewCar: function(){
      let newCar = new Car(this.maxWeight);

      if(parseInt(this.maxWeight) === 0 || this.maxWeight){
        this.$emit('saveNewCar', newCar);
      }
    },

    cancel: function(){
      this.$emit('hideAddNewCarPopup')
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="sass">
body
  overflow: hidden

.darkBg
  width: 100%
  height: 100vh
  background: #00000040
  position: fixed
  top: 0
  left: 0
  overflow: hidden
  display: flex

.popup
  padding: 10px;
  border: 1px solid #bababa
  display: inline-block
  border-radius: 5px
  background: #fff
  margin: auto

.inputWrp
  display: flex

  input
    display: block
    width: 150px
    margin: 15px 4px
    border: 1px solid #bababa;
    font-size: 14px
    padding: 4px

.btnWrp
  display: flex
  justify-content: center

.btn
  padding: 8px 20px
  background: #ccc
  border-radius: 5px
  margin: 0 4px

.add
  background: #42b983
  color: #fff
</style>