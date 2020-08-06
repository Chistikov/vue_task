<template>
  <div>
    <AddBoxPopup v-on:saveNewBox="addNewBox" v-on:hideAddNewBoxPopup="hideAddNewBoxPopup" v-if="addBoxPopupIsVisible"></AddBoxPopup>
    <AddCarPopup v-on:saveNewCar="addNewCar" v-on:hideAddNewCarPopup="hideAddNewCarPopup" v-if="addCarPopupIsVisible"></AddCarPopup>
    <div class="Logistic">
      <div class="container">
        <div class="taskLable">Task 1</div>
        <div class="title">Boxes</div>
        <div>{{ box1 }}</div>
        <div>{{ box2 }}</div>
        <div>{{ box3 }}</div>
      </div>

      <div class="container">
        <div class="taskLable">Task 2</div>
        <div class="title">
          Boxes
          <span class="addBtn" v-on:click="showAddBoxPopup()">+</span>
        </div>
        <div v-for="(item, index) in boxArray" :key="index">
          {{ item }}
          <span class="removeBtn" v-on:click="removeBox(index)">-</span>
        </div>
        <div v-if="boxArray.length != 0">
          <div class="title">Max height: {{ getMaxHeight }} kg</div>
          <div class="title">Max width: {{ getMaxWidth }} kg</div>
          <div class="title">Max depth: {{ getMaxDepth }} kg</div>
        </div>
        <div class="title" v-else>Box array is empty</div>
      </div>

      <div class="container">
        <div class="taskLable">Task 3</div>
        <div class="title">
          Cars
          <span class="addBtn" v-on:click="showAddCarPopup()">+</span>
        </div>
        <div v-for="(item, index) in carsArray" :key="index">{{ item }} <span class="removeBtn" v-on:click="removeCar(index)">-</span></div>

        <div class="title">Total box weight: {{ calcTotalWeight }} kg</div>
        <div class="title" v-if="carsArray.length !== 0 && getSuitableCar">Best car: {{ getSuitableCar }}</div>
        <div class="title" v-else-if="carsArray.length === 0">Car list is empty.</div>
        <div class="title" v-else-if="carsArray.length !== 0 && getSuitableCar === null">Suitable car not found</div>
      </div>
    </div>
  </div>
</template>

<script>
import AddBoxPopup from '@/components/AddBoxPopup.vue'
import AddCarPopup from '@/components/AddCarPopup.vue'

import Car from '@/scripts/Car.js';
import Box from '@/scripts/Box.js';

export default {
  name: "Logistic",
  components: { AddBoxPopup, AddCarPopup},

  data: () => {
    return {
      box1: null,
      box2: null,
      box3: null,
      boxArray: [],
      carsArray: [],
      addBoxPopupIsVisible: false,
      addCarPopupIsVisible: false
    };
  },

  methods: {
    removeBox: function(index){
      this.boxArray.splice(index, 1);
    },

    removeCar: function(index){
      this.carsArray.splice(index, 1);
    },

    showAddBoxPopup: function(){
      this.addBoxPopupIsVisible = true;
    },

    showAddCarPopup: function(){
      this.addCarPopupIsVisible = true;
    },

    hideAddNewCarPopup: function(){
      this.addCarPopupIsVisible = false;
    },

    hideAddNewBoxPopup: function(){
      this.addBoxPopupIsVisible = false;
    },

    addNewBox: function(newBox){
      this.boxArray.push(newBox);
      this.addBoxPopupIsVisible = false;
    },

    addNewCar: function(newCar){
      this.carsArray.push(newCar);
      this.addCarPopupIsVisible = false;
    },

    findItemWithExtremeValues: function(arr, prop, searchValue) {
      let extremeValue;
      let boxItems = [];

      arr.forEach((box) => {
        if (!extremeValue) {
          extremeValue = box[prop];
          boxItems = [box];
        }

        if (searchValue === "max") {
          if (box[prop] > extremeValue) {
            extremeValue = box[prop];
            boxItems = [box];
          } else if (box[prop] === extremeValue) {
            boxItems.push(box);
          }
        }

        if (searchValue === "min") {
          if (box[prop] < extremeValue) {
            extremeValue = box[prop];
            boxItems = [box];
          } else if (box[prop] === extremeValue) {
            boxItems.push(box);
          }
        }
      });

      return boxItems;
    }
  },

  computed: {
    calcTotalWeight: function() {
      let totalWeight = 0;
      this.boxArray.forEach((box) => {
        totalWeight += box.weight;
      });
      return totalWeight;
    },

    getMaxHeight: function(){
      return this.findItemWithExtremeValues(this.boxArray, 'height', 'max');
    },

    getMaxWidth: function(){
      return this.findItemWithExtremeValues(this.boxArray, 'width', 'max');
    },

    getMaxDepth: function(){
      return this.findItemWithExtremeValues(this.boxArray, 'depth', 'max');
    },

    getSuitableCar: function() {
      let suitableCar = null;
      let carsArrayClone = this.carsArray.slice();
      let sortedCarsArr = carsArrayClone.sort((a, b) => {
        if (a.maxWeight > b.maxWeight) {
          return 1;
        }
        if (a.maxWeight < b.maxWeight) {
          return -1;
        }
        return 0;
      });

      for (let i = 0; i < sortedCarsArr.length; i++) {
        if(this.calcTotalWeight <= sortedCarsArr[i]['maxWeight']){
          suitableCar = sortedCarsArr[i];
          break;
        }
      }

      return suitableCar;
    }
  },

  created: function() {
    this.box1 = new Box(10, 20, 30, 10);
    this.box2 = new Box(100, 200, 300, 1000);
    this.box3 = new Box(100000, 2000000, 50000, 0.1);

    this.boxArray.push(new Box(10, 110, 31, 100));
    this.boxArray.push(new Box(20, 120, 32, 101));
    this.boxArray.push(new Box(30, 130, 33, 102));
    this.boxArray.push(new Box(40, 140, 34, 103));
    this.boxArray.push(new Box(50, 150, 35, 104));
    this.boxArray.push(new Box(60, 160, 36, 105));
    this.boxArray.push(new Box(70, 170, 37, 106));
    this.boxArray.push(new Box(80, 180, 38, 107));
    this.boxArray.push(new Box(90, 190, 39, 108));
    this.boxArray.push(new Box(80, 200, 10, 50));

    this.carsArray.push(new Car(900));
    this.carsArray.push(new Car(930));
    this.carsArray.push(new Car(950));
    this.carsArray.push(new Car(1000));
    this.carsArray.push(new Car(1250));
  }
};
</script>

<style lang="sass" scoped>
.container
  border-radius: 5px
  padding: 10px
  padding-bottom: 16px
  margin-bottom: 30px
  border: 1px solid #bababa
  color: #999

.taskLable
  font-size: 20px
  font-weight: 500
  color: #999
  border-bottom: 1px solid #bababa
  padding-bottom: 10px

.title
  font-size: 16px
  font-weight: 500
  margin-bottom: 16px
  padding-top: 16px
  color: #000

.removeBtn
  color: red
  font-weight: 700
  cursor: pointer

.addBtn
  color: #42B983
  font-weight: 700
  cursor: pointer
</style>
