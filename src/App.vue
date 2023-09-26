<script setup>
import { ref } from 'vue'

var display = ref('0')
var equationFormat = ref('')
const symbols = ['+','-','÷','×','.'];

function calc(param) {

  if (symbols.includes(display.value.slice(-1))) {
    display.value = display.value.slice(0, -1);
  }

  for (let i = 0; i < display.value.length; i++) {

    let char = display.value[i];

    switch (char) {
      case '÷':
        equationFormat.value += '/';
        break;
      case '×':
        equationFormat.value += '*';
        break;
      default:
        equationFormat.value += char;
    }

  }

  var result = eval(equationFormat.value);

  if (param == 'negative' && result != 0) {
    result *= -1;
  }

  display.value = result.toString().slice(0, 12);

  if (display.value == 'NaN'){
    display.value = '0'
  }

  equationFormat.value = '';

}

function addNum(x) {

  if(display.value == 'Infinity' || display.value == '-Infinity'){
    display.value = '0';
  }

  if (display.value == '0') {
    display.value=x.toString();
  }

  else if (display.value.length < 12)
    display.value+=x.toString();


}

function addChar(x) {

  if(display.value == 'Infinity' || display.value == '-Infinity' ){
    display.value = '0'+x;
  }

  if (symbols.includes(display.value.slice(-1))) {
    display.value = display.value.slice(0,-1) + x;
  }

  else if (display.value.length < 12)
    display.value+=x.toString();
}

function clear(){
  display.value='0'
}

function clearEntry() {

  while (!symbols.includes(display.value.slice(-1)) && display.value.length > 0) {
    display.value = display.value.slice(0,-1);
  }

  display.value = display.value.slice(0,-1);

  if (display.value == '') {
    display.value = '0';
  }

}

function back(){

  if (display.value ==  'Infinity' || display.value == '-Infinity') {
    display.value = '0';
  }
  else display.value = display.value.slice(0,-1);

  if (display.value == ''){
    display.value = '0';
  }

}

</script>




<template>

  <div class="shape">

    <div class="text-display">
      <div
          contenteditable="false"
          v-text="display"
      ></div>
    </div>

    <div class="keyboard">

      <div>
        <button id="1" @click="addNum(1)" class="buttons" >1</button>
        <button id="2" @click="addNum(2)" class="buttons">2</button>
        <button id="3" @click="addNum(3)" class="buttons">3</button>
        <button id="CE" @click="clearEntry" class="buttons">CE</button>
      </div>


      <div>
        <button id="4" @click="addNum(4)" class="buttons" >4</button>
        <button id="5" @click="addNum(5)" class="buttons" >5</button>
        <button id="6" @click="addNum(6)" class="buttons" >6</button>
        <button id="C" @click="clear" class="buttons" >C</button>
      </div>


      <div>
        <button id="7" @click="addNum(7)" class="buttons" >7</button>
        <button id="8" @click="addNum(8)" class="buttons" >8</button>
        <button id="9" @click="addNum(9)" class="buttons" >9</button>
        <button id="back" @click="back" class="button-back">⌫</button>
      </div>

      <div>
        <button id="0" @click="addNum(0)" class="buttons" >0</button>
        <button id="plus" @click="addChar('+')" class="buttons" >+</button>
        <button id="minus" @click="addChar('-')" class="buttons" >-</button>
        <button id="±" @click="calc('negative')" class="buttons" >±</button>
      </div>

      <div>
        <button id="divide" @click="addChar('÷')" class="buttons" >÷</button>
        <button id="multiply" @click="addChar('×')" class="buttons" >×</button>
        <button id="." @click="addChar('.')" class="buttons" >.</button>
        <button id="=" @click="calc('')" class="equals-button">=</button>
      </div>

    </div>

  </div>

</template>



<style>

.shape {
  width: 200px;
  height: 200px;
  background: linear-gradient(106.5deg, rgba(255, 215, 185, 0.91) 23%, rgba(223, 159, 247, 0.8) 93%);
  border-radius: 10px;
  border: 1px solid white;
}

.text-display {
  font-size: 25px;
  font-family: "Courier New";
  font-weight: bold;
  color: purple;
  position: relative;
  top: 12%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 96%;
  height: 20%;
  background: rgb(255,255,255);
  background: linear-gradient(90deg, rgba(255,255,255,0.9948354341736695) 0%, rgba(233,233,233,1) 50%, rgba(219,218,218,1) 100%);
  border: 1px solid grey;
}

.keyboard {
  position: relative;
  top: 3%;
  left: 2%;
  width: 96%;
  border: solid white;
  border-width: 2px;
  border-radius: 5px 5px 5px 5px;
}

.buttons {
  position: relative;
  width:24%;
  height:25px;
  left:2%;
  color: mediumpurple;
  font-family:  "Courier New";
  font-weight: bold;
  border-radius: 10px;
  background: rgb(249,206,225);
  background: linear-gradient(180deg, rgba(249,206,225,1) 0%, rgba(255,255,255,1) 100%);
  border: 1px solid mediumpurple;
}

.buttons:hover {
  background: white;
}

.buttons:active {
  background: purple;
  color: white;
}

.button-back {
  position: absolute;
  top: 42.1%;
  width:24%;
  height: 24px;
  left:74%;
  color: mediumpurple;
  font-family:  "Courier New";
  font-weight: bold;
  border-radius: 10px;
  background: rgb(249,206,225);
  background: linear-gradient(180deg, rgba(249,206,225,1) 0%, rgba(255,255,255,1) 100%);
  border: 1px solid mediumpurple;
}

.button-back:hover {
  background: white;
}

.button-back:active{
  background: purple;
}


.equals-button {
  position: relative;
  background: rgb(63,94,251);
  background: linear-gradient(180deg, rgba(63,94,251,1) 0%, rgba(152,83,183,1) 47%, rgba(252,70,107,1) 100%);
  font-family:  "Courier New";
  font-weight: bold;
  color: white;
  width:24%;
  height: 24px;
  left:2%;
  border-radius: 10px;
  border: 1px solid white;
}

.equals-button:hover {
  background: rebeccapurple;
}

.equals-button:active {
  background: purple;
}

</style>