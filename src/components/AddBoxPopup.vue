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
export default {
  props: {},

  data: ()=>(
    {
      width: undefined,
      height: undefined,
      depth: undefined,
      weight: undefined,
    }
  ),
  
  methods: {
    addNewBox: function(){
      function createBox(width, height, depth, weight) {
        this.width = +width;
        this.height = +height;
        this.depth = +depth;
        this.weight = +weight;
      }

      let newBox = new createBox(this.width, this.height, this.depth, this.weight);

      if(this.width && this.height && this.depth && this.weight){
        this.$emit('saveNewBox', newBox);
      }
    },

    cancel: function(){
      this.$emit('hideAddNewBoxPopup')
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
  // width: 400px

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