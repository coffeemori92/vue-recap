<template>
  <div>
    <div>당첨 숫자</div>
    <div id="result">
      <lotto-ball v-for="ball in winBalls" :key="ball" :number="ball"/>
    </div>
    <div>보너스</div>
    <lotto-ball v-if="bonus" :number="bonus" />
    <button v-if="redo" @click="onClickRedo">한 번 더!</button>
  </div>
</template>

<script>
import LottoBall from './LottoBall';

const getWinNumbers = () => {
  console.log('getWinNumbers');
  const candidates = Array(45).fill().map((v, i) => i + 1);
  const shuffle = [];
  while(candidates.length > 0) {
    shuffle.push(candidates.splice(Math.floor(Math.random() * candidates.length), 1)[0]);
  }
  const bonousNumber = shuffle[shuffle.length - 1];
  const winNumbers = shuffle.slice(0, 6).sort((a, b) => a - b);
  return [...winNumbers, bonousNumber];
};

const timeouts = [];

export default {
  components: { LottoBall },
  data() {
    return {
      winNumbers: getWinNumbers(),
      winBalls: [],
      bonus: null,
      redo: false,
    };
  },
  computed: {
    
  },
  methods: {
    onClickRedo() {
      this.winNumbers = getWinNumbers();
      this.winBalls = [];
      this.bonus = null;
      this.redo = false;
      this.showBalls();
    },
    showBalls() {
      for(let i = 0; i < this.winNumbers.length - 1; i ++) {
        timeouts[i] = setTimeout(() => {
          this.winBalls.push(this.winNumbers[i]);
        }, (i + 1) * 1000);
      }
      timeouts[6] = setTimeout(() => {
        this.bonus = this.winNumbers[6];
        this.redo = true;
      }, 7000);
    }
  },
  created() {
    console.log('created');
  },
  mounted() {
    console.log('mounted');
    this.showBalls();
  },
  updated() {
    console.log('updated');
  },
  beforeDestroy() {
    console.log('beforeDestroy');
    timeouts.forEach(t => clearTimeout(t))
  },
  destroyed() {
    console.log('destroyed');
  },
  watch: {
    // winBalls(val, oldVal) {
    //   if(val.length === 0) {
    //     this.showBalls();
    //   }
    // }
  },
}
</script>

<style scoped>

</style>