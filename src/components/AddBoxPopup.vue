<template>
  <div class="darkBg">
    <div class="popup">
      <div>Adding new box</div>
      <div class="inputWrp">
        <input type="text" placeholder="width (sm)" v-model="width">
        <input type="text" placeholder="height (sm)" v-model="height">
        <input type="text" placeholder="depth (sm)" v-model="depth">
        <input type="text" placeholder="weight (sm)" v-model="weight">
      </div>
      <div class="btnWrp">
        <div class="btn cancel" v-on:click="cancel">cancel</div>
        <div class="btn add" v-on:click="addNewBox">add</div>
      </div>
    </div>
  </div>
</template>

<script>
import Box from '@/scripts/Box.js';

export default {
  data: ()=>(
    {
      width: null,
      height: null,
      depth: null,
      weight: null,
    }
  ),
  
  methods: {
    addNewBox: function(){
      let newBox = new Box(this.width, this.height, this.depth, this.weight);
      debugger;
      if((parseInt(this.width) === 0 || this.width) && (parseInt(this.height) === 0 || this.height) && (parseInt(this.depth) === 0 || this.depth) && (parseInt(this.weight) === 0 ||this.weight)){
        this.$emit('saveNewBox', newBox);
      }
    },

    cancel: function(){
      this.$emit('hideAddNewBoxPopup')
    }
  }
};
</script>

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
    width: 80px
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