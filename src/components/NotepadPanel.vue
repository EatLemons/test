<template>
  <div id="notepadPanel-panel">
    <div class="topPanel">
      <div class="input-area">
        <div  v-for="item in inputingDatas" :key="item.key" class="inputingDatas-item">
        {{ item.val }}
      </div>
      <input type="text" @keydown="handleInput" v-model.trim="inputData" />
      </div>
      <button @click="add">新 增</button>
    </div>
    <div  class="bottomPanel">
      <div v-for="item in showData" :key="item.key" class="bottomPanel-item"  @mouseenter="handleMouseEnter(item)" @mouseleave="handleMouseLeave()">
        <div class="content" :class="item.statue === 1 ? 'done' : ''">
          {{ item.val }} {{ item.statue === 0 ? '' : '√'}}
        </div>
        <div class="operation" v-if="currentKey === item.key">
          <button @click="handleDel(item.key)">删除</button>
          <button @click="handleDone(item.key)">完成</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
name: 'NotepadPanel',
setup() {

  const inputData = ref('');
  const inputingDatas = ref([]);
  const showData = ref( [
        // { key:'1', val: 'C', statue: 0 }, 
      ]);
    const currentKey = ref(undefined)
  function handleMouseEnter(item) {
    currentKey.value = item.key;
    console.log(item);
  }
function handleMouseLeave() {
  currentKey.value = undefined;
}
function handleDel(key) {
  showData.value = showData.value.filter(item => item.key !== key)
}
function handleDone(key) {
  showData.value = showData.value.map(item => {
    if (item.key === key) {
      item.statue = 1;
    }
    return item;
  })
}
function handleInput(e) {
  if(inputData.value === '') {
    return;
  }
  if (e.keyCode === 13) {
    inputingDatas.value.push({ key: Date.now(), val: inputData.value, statue: 0 });
    inputData.value = '';
  }
}
function add() {
  if(inputData.value !== '') {
    inputingDatas.value.push({ key: Date.now(), val: inputData.value, statue: 0 });
  }
  showData.value.push(...inputingDatas.value);
  inputingDatas.value = []
  inputData.value = '';
}
  return {
    inputData,
    showData,
    currentKey,
    handleMouseEnter,
    handleMouseLeave,
    handleDel,
    handleDone,
    handleInput,
    inputingDatas,
    add
  }
},
}
</script>

<style scoped>
#notepadPanel-panel {
width: calc(100% - 4vh);
height: calc(100% - 4vh);
margin: 2vh;
}
.topPanel{
width: 100%;
height: 20%;
margin: .3vh auto;
display: flex;
justify-content: space-between;
align-items: center;
}
/* .topPanel button{
  width: 13%;
  height: 82%;
  cursor: pointer;
  font-size: 2vh;
  padding: 0.5rem;
  background-image: linear-gradient(
    0deg,
    rgba(94, 58, 238, 1) 0%,
    rgba(197, 107, 240, 1) 100%
  );

  color: var(--white);

  border: 0;
  border-radius: 10vh;
  box-shadow: inset 0 -2px 25px -4px var(--white);
} */
.input-area::-webkit-scrollbar {
      width: 1vh; 
   }

   .input-area::-webkit-scrollbar-track {
    background-color: rgba(0, 0, 0, 0);
  }

  .input-area::-webkit-scrollbar-thumb {
    background: #4e4e4e42;
    border-radius: 1vh;
  }
.input-area{
border: .2vh solid;
  width: 85%;
  height: 80%;
  display: flex;
  flex-wrap: wrap;
  overflow: auto;
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
.topPanel input{
  border: none;
  outline: none;
  color: #fff;
  font-size: 2vh;
  background-color: #ffffff10;
  width: 100%;
}
.topPanel input:focus {
  outline: none;
}

.inputingDatas-item{
  height: 3.9vh;
  border: .1vh solid;
  border-radius: 3vh;
  color: rgba(252, 255, 255, 0.978);
  border-color: rgba(252, 255, 255, 0.418);
  margin: 1vh;
  padding: 0 2vh;
  max-width: 10vh;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
.bottomPanel{
width: 100%;
height: 80%;
border: .2vh solid;
border-color: rgba(252, 255, 255, 0.418);
overflow: auto;
}
.bottomPanel::-webkit-scrollbar {
      width: 1vh; 
   }

   .bottomPanel::-webkit-scrollbar-track {
    background-color: rgba(0, 0, 0, 0);
  }

  .bottomPanel::-webkit-scrollbar-thumb {
    background: #4e4e4e42;
    border-radius: 1vh;
  }
.bottomPanel-item{
  width: 90%;
  height: 4vh;
  margin: .4vh auto;
  display: flex;
  justify-content: space-between;
}

.bottomPanel-item .content{
  border: .3vh solid;
  border-color: rgba(252, 255, 255, 0.418);
  border-radius: 1vh;
  padding: 0 2vh;
  max-width: 40vh;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  line-height: 3.9vh;
  background-color: rgba(246, 106, 251, 0.3);
  color: #ffffffc1;
}
.bottomPanel .done{
  color: #ffffff34;
  background-color: rgba(246, 106, 251, 0.242) !important;
}
.bottomPanel-item .operation{
  display: flex;
    align-content: center;
    flex-wrap: wrap;
}
.bottomPanel-item .operation button {
  margin: 0 1vh;
  --color: #560bad;
  font-family: inherit;
  display: inline-block;
  width: 8vh;
  height: 4vh;
  line-height: 2em;
  position: relative;
  cursor: pointer;
  overflow: hidden;
  border: .3vh solid var(--color);
  transition: color 0.5s;
  z-index: 1;
  font-size: 2vh;
  border-radius: 2vh;
  font-weight: 500;
  color: var(--color);
}

.bottomPanel-item .operation button:before {
  content: "";
  position: absolute;
  z-index: -1;
  background: var(--color);
  height: 150px;
  width: 200px;
  border-radius: 50%;
}

.bottomPanel-item .operation button:hover {
  color: #fff;
}

.bottomPanel-item .operation button:before {
  top: 100%;
  left: 100%;
  transition: all 0.7s;
}

.bottomPanel-item .operation button:hover:before {
  top: -30px;
  left: -30px;
}

.bottomPanel-item .operation button:active:before {
  background: #3a0ca3;
  transition: background 0s;
}

.topPanel button {
  margin: 0 1vh;
  background-color: #d900ff82;
  --color: #570badc9;
  font-family: inherit;
  display: inline-block;
  width: 15%;
  height: 82%;
  line-height: 3vh;
  position: relative;
  cursor: pointer;
  overflow: hidden;
  border: .3vh solid var(--color);
  transition: color 0.5s;
  z-index: 1;
  font-size: 2.8vh;
  border-radius: 2vh;
  font-weight: 600;
  color: #ffffffb5;
}

.topPanel button:before {
  content: "";
  position: absolute;
  z-index: -1;
  background: var(--color);
  height: 150px;
  width: 200px;
  border-radius: 50%;
}

.topPanel button:hover {
  color: #fff;
}

.topPanel button:before {
  top: 100%;
  left: 100%;
  transition: all 0.7s;
}

.topPanel button:hover:before {
  top: -30px;
  left: -30px;
}

.topPanel button:active:before {
  background: #3a0ca3;
  transition: background 0s;
}
</style>