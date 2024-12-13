<template>
  <div id="Stamp-panel">
    <div>
      <div>邮票A面值</div>
      <input type="number" min="0" v-model="inputA" @keydown="handleInput('A', inputA)">
    </div>
    <div>
      <div>邮票B面值</div>
      <input type="number" min="0" v-model="inputB" @keydown="handleInput('B', inputB)">
    </div>
    <div>
      <div>需要邮资T</div>
      <input type="number" min="0" v-model="inputT" @keydown="handleInput('T', inputT)">
    </div>
    <div>
      <div>最小浪费额度</div>
      <input type="text" readonly disabled  v-model="inputW">
    </div>
    <button  @click="handleComputer()">计算</button>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
name: 'StampPanel',
setup() {

  const inputA = ref('');
  const inputB = ref('');
  const inputT = ref('');
  const inputW = ref('');
  function handleInput(type, value) {
    if (type === 'A') {
      inputA.value = value;
    } else if (type === 'B') {
      inputB.value = value;
    } else if (type === 'T') {
      inputT.value = value;
    }
  }
  function handleComputer() {
    const a = Number(inputA.value);
    const b = Number(inputB.value);
    const t = Number(inputT.value);
    if(a >= 1000000000 || b >= 1000000000 || t >= 1000000000) {
      alert('输入值过大,不能超过1000000000');
    }
    if(a < 0 || b < 0 || t < 0) {
      alert('输入值不能为负数');
    }
    const max = Math.max(a, b);
    const min = Math.min(a, b);

    let inputWValue;
    if(max === 0){
      // AB值都为0
      inputWValue = 'Infinity'
    } else if(min === 0){
    // 较小值为0
      t%max === 0 ? inputWValue = 0 : inputWValue = Math.abs(t%max - max)
    } else {
      //AB值都大于0

      // 向上取整算出只用较小值拼成目标金额需要多少张
      const n = Math.ceil(t/min);
      // 定义浪费为无穷大
      let waste = Infinity
      // 遍历较小值拼成目标金额需要多少张
      for(let i = 0; i <= n; i++) {
        // 计算浪费
        const wasteTemp = Math.abs(t - i*min - Math.ceil((t-i*min)/max)*max)
        // 如果浪费更小则更新浪费
        if(wasteTemp < waste) {
          waste = wasteTemp
        }
      }
      inputWValue = waste

    }
    inputW.value = inputWValue;

  }
  return {
    inputA,
    inputB,
    inputT,
    inputW,
    handleInput,
    handleComputer,
  }
},
}
</script>

<style scoped>
#Stamp-panel {
width: calc(100% - 4vh);
height: calc(100% - 4vh);
margin: 2vh;
color: white;
user-select: none;
}
#Stamp-panel div{
  width: 80%;
  margin: 1vh auto;
  display: flex;
  justify-content: space-between;
}
#Stamp-panel  button{
  display: block;
  width: 50%;
  height: 4vh;
  margin: 4vh auto;
  cursor: pointer;
}

</style>