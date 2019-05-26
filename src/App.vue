<template>
  <div id="app">
    <div class="wrapper">
      <div class="field">
        <div :class="['cell', openedColors[i - 1]]" v-for="i in 16" :key="i" @click="openCell(i - 1)"></div>
      </div>
      <Timer @start="start" :win="win" @time="asd"/>
    </div>
  </div>
</template>

<script>
import Timer from './components/Timer'

export default {
  data() {
    return {
      colors: ['red', 'blue', 'green', 'yellow', 'orange', 'gray', 'pink', 'cyan', 'red', 'blue', 'green', 'yellow', 'orange', 'gray', 'pink', 'cyan'],
      openedColors: [],
      openedId: [],
      previousColor: '',
      previousId: -1,
      win: false,
      started: false
    }
  },
  components: {
    Timer
  },
  name: 'app',
  methods: {
    shuffle() {
      var newColors = this.colors;
      this.colors = [];
      for (var i = newColors.length - 1; i > 0; i--) {
        var num = Math.floor(Math.random() * (i + 1));
        var d = newColors[num];
        newColors[num] = newColors[i];
        newColors[i] = d;
      }
      this.colors = newColors;
    },
    start() {
      this.shuffle();
      this.openedColors = [];
      this.openedId = [],
      this.reset();
      this.win = false;
      this.started = true;
    },
    openCell(i) {
      if (i == this.previousId || this.openedId.includes(i) || this.openedId.length == this.colors.length || !this.started)
        return;

      if (this.previousColor == '') {
        this.previousColor = this.colors[i];
        this.previousId = i;
        this.paintCell(i);
      } else if (this.previousColor == this.colors[i]) {
        this.paintCell(i);
        this.openedId.push(i);
        this.openedId.push(this.previousId);
        this.reset();
        if (this.openedId.length == this.colors.length) {
          this.win = true;
          console.log(this.win);
        }
      } else {
        this.deleteCell();
        this.reset();
      }
    },
    paintCell(i) {
      this.openedColors[i] = this.colors[i];
      var temp = this.openedColors;
      this.openedColors = [];
      this.openedColors = temp;
    },
    deleteCell() {
      this.openedColors[this.previousId] = '';
      var temp = this.openedColors;
      this.openedColors = [];
      this.openedColors = temp;
    },
    reset() {
      this.previousColor = '';
      this.previousId = -1;
    },
    asd(time) {
      alert('Вы победили! Ваше время - ' + time);
    }
  }
}
</script>

<style lang="scss" scoped>
  .wrapper {
    width: 402px;
    margin: 0 auto;
  }

  .field {
    width: 400px;
    height: 400px;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
  }

  button {
    margin-top: 10px;
  }

  .cell {
    border: 1px solid gray
  }
</style>

<style lang="scss">
  .red { 
    background-color: red; 
  }

  .blue { 
    background-color: blue; 
  }

  .green {
    background-color: green; 
  }

  .yellow {
    background-color: yellow; 
  }

  .orange {
    background-color: orange; 
  }

  .gray {
    background-color: gray; 
  }

  .pink {
    background-color: pink; 
  }

  .cyan {
    background-color: cyan; 
  }
</style>
