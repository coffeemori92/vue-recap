<template>
  <div>
    <div 
      id="computer" 
      :style="computedStyleObject" 
      :class="{state: true}"></div>
    <div>
      <button @click="onClickButton('R')">바위</button>
      <button @click="onClickButton('C')">가위</button>
      <button @click="onClickButton('P')">보</button>
    </div>
    <div>{{result}}</div>
    <div>현재 {{score}}점</div>
  </div>
</template>

<script>
const rspCoords = {
  R: '0',
  C: '-142px',
  P: '-284px',
};
const scores = {
  R: 1,
  C: 0,
  P: -1,
};
const computerChoice = imgCoord => {
  return Object.entries(rspCoords).find(v => v[1] === imgCoord)[0];
};
let interval = null;
export default {
  data() {
    return {
      result: '',
      score: 0,
      imgCoord: rspCoords.R,
    };
  },
  computed: {
    computedStyleObject() {
      return {
        background: `url(https://en.pimg.jp/023/182/267/1/23182267.jpg) ${this.imgCoord} 0`
      };
    }
  },
  methods: {
    changeHand() {
      interval = setInterval(() => {
        if(this.imgCoord === rspCoords.R) {
          this.imgCoord = rspCoords.C;
        } else if(this.imgCoord === rspCoords.C) {
          this.imgCoord = rspCoords.P;
        } else if(this.imgCoord === rspCoords.P) {
          this.imgCoord = rspCoords.R;
        }
      }, 100);
    },
    onClickButton(choice) {
      clearInterval(interval);
      const myScore = scores[choice];
      const cpuScore = scores[computerChoice(this.imgCoord)];
      const diff = cpuScore - myScore;
      if(diff === 0) {
        this.result = '비겼습니다.';
      } else if([-1, 2].includes(diff)) {
        this.result = '이겼습니다.';
        this.score += 1;
      } else {
        this.result = '졌습니다.';
        this.score -= 1;
      }
      setTimeout(() => {
        this.changeHand();
      }, 1000);
    },
  },
  created() {
    console.log('created');
  },
  mounted() {
    console.log('mounted');
    this.changeHand();
  },
  updated() {
    console.log('updated');
  },
  beforeDestroy() {
    console.log('beforeDestroy');
    clearInterval(interval);
  },
  destroyed() {
    console.log('destroyed');
  },
}
</script>

<style scoped>
  #computer {
    width: 142px;
    height: 200px;
    background-position: 0 0;
  }
</style>