<template>
  <div>
    <p>{{m + ':' + s + ':' + ms}}</p>
    <button @click="start">Start</button>
  </div>
</template>

<script>
import { setInterval, clearInterval } from 'timers';
export default {
  data() {
    return {
        go: false,
        m: '00',
        s: '00',
        ms: '000',
        interval: null
    }
  },
  props: ['win'],
  name: 'Timer',
  methods: {
    start() {
        
        clearInterval(this.interval);
        this.$emit('start');
        this.m = '00';
        this.s = '00';
        this.ms = '000';
        var start = new Date().getTime();
        this.interval = setInterval(() => {
            var now = new Date().getTime();
            var elapsed = now - start;
            this.ms = elapsed;

            if (this.ms > 999) {
                start = new Date().getTime();
                this.ms = '000';
                if (this.s == 59) {
                    this.s = '00';
                    this.m++;
                    if (this.m < 10)
                        this.m = '0' + this.m;
                } else {
                    this.s++;
                    if (this.s < 10)
                        this.s = '0' + this.s;
                }
            }            
        }, 25);
    }
  },
  watch: {
      win: function() {
        if (this.win) {
            this.$emit('time', this.m + ':' + this.s + ':' + this.ms);
            clearInterval(this.interval);
        }
      }
  }
  
}
</script>

<style lang="scss" scoped>

</style>
