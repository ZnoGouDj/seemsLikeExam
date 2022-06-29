<template>
  <div :class="{ paddingTop: isRequired }">
    <div class="main">
      <select-buttons @onChange="changeFilter"></select-buttons>
      <h1>Interview Random Topic Generator</h1>
      <h2 v-if="fetchedTopics.length">{{ fetchedTopics.length }} questions left</h2>
      <div class="currentTopic">{{ currentTopic }}</div>
      <my-button v-if="!isStarted" @click="fetchTopics">Generate Random Topic</my-button>
      <my-button v-else @click="selectNewTopic">Next topic</my-button>

      <div>
        {{ timer.minutes < 10 ? '0' + timer.minutes : timer.minutes }}:{{
          timer.seconds < 10 ? '0' + timer.seconds : timer.seconds
        }}
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      timer: {
        minutes: 5,
        seconds: 0,
        start: true,
        interval: '',
      },
      isRequired: window.innerHeight > 500,
      fetchedTopics: [],
      currentTopics: [],
      filteredTopics: 'All',
      currentTopic: 'click 👇 to start ',
      isStarted: false,
    };
  },
  methods: {
    changeFilter(topic) {
      console.log(topic);
      // this.filteredTopics = topic;
      // this.stopTimer();
      // this.fetchTopics();
      // this.isStarted = false;
    },
    setTopic() {
      let rnd = Math.floor(Math.random() * this.fetchedTopics.length);
      this.currentTopic = this.fetchedTopics[rnd];
      console.log(this.fetchedTopics[0]); //? and fix here
      this.fetchedTopics.splice(rnd, 1);
    },
    startTimer() {
      let t = this.timer;
      t.start = false;
      t.interval = setInterval(() => {
        if (t.seconds === 0 && t.minutes === 0) {
          this.stopTimer();
        } else if (t.seconds === 0) {
          t.minutes--;
          t.seconds = 59;
        } else {
          t.seconds--;
        }
      }, 1000);
    },
    stopTimer() {
      clearInterval(this.timer.interval);
      this.timer = {
        minutes: 5,
        seconds: 0,
        start: true,
        interval: '',
      };
    },
    selectNewTopic() {
      this.stopTimer();
      this.startTimer();
      this.setTopic();
    },
    async fetchTopics() {
      try {
        const response = await axios.get('https://62a0f78a7b9345bcbe4358a7.mockapi.io/questions');
        let arr = [];

        response.data.forEach(el => {
          for (let key in el) {
            let elem = el[key];
            arr.push(elem);
          }
        });

        if (this.filteredTopics === 'All') {
          let allTopics = {};

          arr.forEach((el, index) => {
            for (let key in el) {
              if (!allTopics[index]) {
                allTopics[index] = [el[key]];
              } else {
                allTopics[index].push(el[key]);
              }
            }
          }); //! it works here
          this.fetchedTopics = allTopics;
          this.startTimer();
        } else {
          let topicFilter = [];
          let el = arr[this.filteredTopics];
          for (let key in el) {
            topicFilter.push(el[key]);
          }
          this.fetchedTopics = topicFilter;
        } //? fix here

        this.setTopic();
        this.isStarted = true;
      } catch (e) {
        console.log(e);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
* {
  color: #e3ce0f;
}

.paddingTop {
  padding-top: calc(100vh / 5);
}

.main {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 30px 20px;

  div {
    margin: 30px 0;
    font-size: 3em;
  }

  h2 {
    margin-top: 30px;
  }

  .currentTopic {
    padding: 80px 100px;
    border-radius: 10px;
    background-color: black;
  }
}
</style>
