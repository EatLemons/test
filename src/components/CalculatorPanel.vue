<template>
    <div id="calculator-panel" >
      <div class="topPanel"><input type="text" v-model="showData" readonly @keydown="handleKeyDown($event.key)"></div>
      <div  class="bottomPanel">
        <div class="leftPanel">
          <div class="leftPanel-item" v-for="item in leftItems" :key="item.val" @click="handleClick(item.val)">
              {{ item.val }}
          </div>
          <div  class="leftPanel-last-item" @click="handleEqual()"> = </div>
        </div>
        <div class="rightPanel">
          <div class="rightPanel-item" v-for="item in rightItems" :key="item.val" @click="handleComputer(item.val)" >
              {{ item.val }}
          </div>
        </div>
      </div>
    </div>
</template>

<script>
import { ref } from 'vue';

export default {
  name: 'CalculatorPanel',
  setup() {
    const showData = ref('');
    const leftItems = ref(
      [
        { val: 'C' }, 
        { val: '1' }, 
        { val: '2' }, 
        { val: '3' }, 
        { val: '4' }, 
        { val: '5' }, 
        { val: '6' }, 
        { val: '7' }, 
        { val: '8' }, 
        { val: '9' }, 
        { val: '0' },
        { val: '.' },
      ])

      const rightItems = ref(
      [
        { val: '+' }, 
        { val: '-' }, 
        { val: 'x' }, 
        { val: '/' }, 
      ])

      function handleClick(val) {
        if (showData.value === 'error' || showData.value === 'NaN' || showData.value === 'Infinity') {
          showData.value = '';
        }
        if (val === 'C') {
          showData.value = '';
        } else {
          showData.value += val;
        }
      }
      function handleComputer(val) {
        showData.value += val;
      }
      function handleEqual() {
        console.log('equal');
        try {
          showData.value = eval(showData.value).toString();
        } catch (error) {
          showData.value = 'error';
        }
      }
      function handleKeyDown(key) {
        if (key === 'Enter') {
          handleEqual();
        } else if (key === 'Backspace') {
          showData.value = showData.value.slice(0, -1);
        } else if(key>='0' && key <= '9' || key === '+' || key === '-' || key === 'x' || key === '/' || key === '.') {
          handleClick(key);
        }
      }
    return {
      showData,
      leftItems,
      rightItems,
      handleClick,
      handleComputer,
      handleEqual,
      handleKeyDown
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#calculator-panel {
  width: calc(100% - 4vh);
  height: calc(100% - 4vh);
  margin: 2vh;
}
.topPanel{
  width: 100%;
  height: 20%;
  font-weight: bold;
  overflow-x: auto;

}
.topPanel input{
  width: 98%;
  height: 77%;
  margin: 0.6vh 0 0 0 ;
  border: .2vh solid;
  font-size: 3vh;
  outline: none;
  color: #fff;
  text-align: center;
  font-weight: bold;
  user-select: none;
  border-color: rgba(252, 255, 255, 0.418);
  background-color: hsla(240, 15%, 9%, 1);
  background-image: radial-gradient(
      at 88% 40%,
      hsla(240, 15%, 9%, 1) 0px,
      transparent 85%
    ),
    radial-gradient(at 49% 30%, hsla(240, 15%, 9%, 1) 0px, transparent 85%),
    radial-gradient(at 14% 26%, hsla(240, 15%, 9%, 1) 0px, transparent 85%),
    radial-gradient(at 0% 64%, hsla(263, 93%, 56%, 1) 0px, transparent 85%),
    radial-gradient(at 41% 94%, hsla(284, 100%, 84%, 1) 0px, transparent 85%)
}
.bottomPanel{
  display: flex;
  width: 100%;
  height: 80%;
}
.leftPanel{
  width: 80%;
  height: 100%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  text-align: center;
  line-height: 6vh;
}
.leftPanel-item{
  width: 20%;
  height: 20%;
  border: .2vh solid;
  color: rgba(252, 255, 255, 0.709);
  border-color: rgba(252, 255, 255, 0.418);
  margin: .3vh 0;
  cursor: pointer;
  user-select: none;
}
.leftPanel-last-item{
  width: 95%;
  height: 20%;
  border: .2vh solid;
  margin: .3vh 0;
  line-height: 6vh;
  cursor: pointer;
  user-select: none;
  color: rgba(252, 255, 255, 0.709);
  border-color: rgba(252, 255, 255, 0.418);
}
.rightPanel{
  width: 20%;
  height: 100%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  text-align: center;
  line-height: 6vh;
}
.rightPanel-item{
  width: 80%;
  height: 20%;
  border: .2vh solid;
  margin: .3vh 0;
  cursor: pointer;
  user-select: none;
  border-color: rgba(252, 255, 255, 0.418);
  color: rgba(252, 255, 255, 0.709);
}
</style>
