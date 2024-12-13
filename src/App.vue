<template>
  <main>
    <div class="card__border"></div>
    <div id="date">今天是{{ showDate }}    温度：{{ tem }}℃ 天气： {{ weather }}</div>
    <nav>
      <div :class="{ 'active': isActive === 0}" @click="isActive = 0">计算器</div>
      <div :class="{ 'active': isActive === 1}" @click="isActive = 1" >记事本</div>
      <div :class="{ 'active': isActive === 2}" @click="isActive = 2" >邮票</div>
    </nav>
    <div style="width: 100%; height: 70%">
    <Calculator v-show="isActive === 0"/>
    <Notepad  v-show="isActive ===1"/>
    <Stamp  v-show="isActive ===2" />
    </div>
  </main>
</template>

<script>
import Calculator from './components/CalculatorPanel.vue'
import Notepad  from './components/NotepadPanel.vue'
import Stamp  from './components/Stamp.vue'
import { ref } from 'vue';

export default {
  name: 'App',
  components: {
    Calculator,
    Notepad,
    Stamp
  },
  setup() {
   const isActive = ref(0);
   const tem = ref('/');
   const weather = ref('/');
   const currentDate = new Date();
   const year = currentDate.getFullYear();
   const month = currentDate.getMonth() + 1;
   const day = currentDate.getDate();
   const showDate = `${year}年${month}月${day}日`

    return {
      isActive,
      showDate,
      weather,
      tem
    };
  },
  mounted() {
    fetch('https://restapi.amap.com/v3/weather/weatherInfo?key=ff5611295aa433828eeaa40cf98ba3a0&city=610400').then(res => res.json().then(data => {
      this.weather = data.lives[0].weather;
      this.tem = data.lives[0].temperature;
    })).catch(err => {
      console.log(err);
    })
  }
}
</script>

<style>
#app {
  width: 100%;
  height: 100vh;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(45, 41, 41, 0.235);
  font-family: 'Courier New', Courier, monospace;
}
main {
  width: 80vh;
  height: 60vh;
  font-size: 3vh;
  --white: hsl(0, 0%, 100%);
  --black: hsl(240, 15%, 9%);
  --paragraph: hsl(0, 0%, 83%);
  --line: hsl(240, 9%, 17%);
  --primary: hsl(266, 92%, 58%);
  position: relative;


  padding: 1vh;
  background-color: hsla(240, 15%, 9%, 1);
  background-image: radial-gradient(
      at 88% 40%,
      hsla(240, 15%, 9%, 1) 0px,
      transparent 85%
    ),
    radial-gradient(at 49% 30%, hsla(240, 15%, 9%, 1) 0px, transparent 85%),
    radial-gradient(at 14% 26%, hsla(240, 15%, 9%, 1) 0px, transparent 85%),
    radial-gradient(at 0% 64%, hsla(263, 93%, 56%, 1) 0px, transparent 85%),
    radial-gradient(at 41% 94%, hsla(284, 100%, 84%, 1) 0px, transparent 85%),
    radial-gradient(at 100% 99%, hsla(306, 100%, 57%, 1) 0px, transparent 85%);

    border-radius: 1vh;
  box-shadow: 0 -1.6vh 2.4vh 0 rgba(255, 255, 255, 0.25) inset;
}
.card__border {
  overflow: hidden;
  pointer-events: none;

  position: absolute;
  z-index: -10;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);

  width: calc(100% + 1vh);
  height: calc(100% + 1vh);
  background-image: linear-gradient(
    0deg,
    hsl(0, 0%, 100%) -50%,
    hsl(0, 0%, 40%) 100%
  );

  border-radius: 1vh;
}

.card__border::before {
  content: "";
  pointer-events: none;

  position: fixed;
  z-index: 200;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%), rotate(0deg);
  transform-origin: left;

  width: 200%;
  height: 10rem;
  background-image: linear-gradient(
    0deg,
    hsla(0, 0%, 100%, 0) 0%,
    hsl(277, 95%, 60%) 40%,
    hsl(277, 95%, 60%) 60%,
    hsla(0, 0%, 40%, 0) 100%
  );

  animation: rotate 8s linear infinite;
}

@keyframes rotate {
  to {
    transform: rotate(360deg);
  }
}
#date {
  width: 100%;
  height: 20%;
  line-height: 10vh;
  text-align: center;
  color: var(--white);
}
nav{
  width: 100%;
  height: 10%;
  display: flex;
  border-bottom: .3vh solid;
  border-color: #ffffff8e;
  color: var(--white);
}
nav div {
  text-align: center;
  width: 20%;
  height: 100%;
  line-height: 6vh;
  cursor: pointer;
  user-select: none;
  
}
nav div:hover{
  background-color: #b33ec853;
}
nav .active{
  border-bottom: none;
  background-color: #b33ec89f;

}
</style>
