<template>
  <div class="timer">
    Time spent on VueBlog: {{ formattedTime }}
  </div>
</template>

<script>
export default {
  data() {
    return {
      startTime: null,
      formattedTime: "00:00:00",
    };
  },
  mounted() {
    this.startTime = new Date();
    this.updateTime();
  },
  methods: {
    updateTime() {
      setInterval(() => {
        const currentTime = new Date();
        const timeDifference = currentTime - this.startTime;
        const hours = Math.floor(timeDifference / 3600000);
        const minutes = Math.floor((timeDifference % 3600000) / 60000);
        const seconds = Math.floor((timeDifference % 60000) / 1000);
        this.formattedTime = `${this.formatTime(hours)}:${this.formatTime(minutes)}:${this.formatTime(seconds)}`;
      }, 1000);
    },
    formatTime(value) {
      return value < 10 ? `0${value}` : value;
    },
  },
};
</script>

<style scoped>
.timer {
  position: fixed;
  bottom: 10px;
  right: 10px;
  background-color: #007bff;
  color: #fff;
  padding: 5px 10px;
  border-radius: 5px;
  font-size: 16px;
}
</style>
