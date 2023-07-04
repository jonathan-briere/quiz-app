<template>
  <div></div>
</template>

<script>
export default {
  data() {
    return {
      timer: null,
      totalTime: 20 * 60
    };
  },
  // ========================
  methods: {
    startTimer: function() {
      this.timer = setInterval(() => this.countdown(), 1000);
    },

    stopTimer: function() {
      clearInterval(this.timer);
      this.timer = null;
    },

    padTime: function(time) {
      return (time < 10 ? "0" : "") + time;
    },
    countdown: function() {
      if (this.totalTime >= 1) {
        this.totalTime--;
      } else {
        this.totalTime = 0;
      }
      this.sendTime();
    },

    sendTime: function() {
      this.$emit("sendTime", this.minutes + ":" + this.seconds);
    },

    stopper: function() {
      this.$emit("stopTimer", this.stopTimer);
    }
  },
  // ========================
  computed: {
    minutes: function() {
      const minutes = Math.floor(this.totalTime / 60);
      return this.padTime(minutes);
    },
    seconds: function() {
      const seconds = this.totalTime - this.minutes * 60;
      return this.padTime(seconds);
    }
  },

  mounted() {
    this.startTimer();
    this.stopper();
  }
};
</script>

<style scoped>
.timer {
  position: relative;
  top: -7px;
  padding-left: 5px;
}
</style>
