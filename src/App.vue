<template>
  <div class="pop-up-block" @click="changeFilter()">
    <div class="main-container">
      <div class="main" @click.stop="workaround">
        <h1>Interview Random Topic Generator</h1>
        <select-buttons @onChange="changeFilter" :isSelected="isTopicSelected"></select-buttons>
        <div class="current-topic">
          <div class="topics-amount" v-if="topicsAmount && currentTopics.length">
            {{ topicsAmount }}/{{ currentTopics.length + topicsAmount }}
          </div>
          <p>{{ currentTopic }}</p>
          <div class="timer" v-if="topicsAmount && currentTopics.length">
            {{ timer.minutes < 10 ? '0' + timer.minutes : timer.minutes }}:{{
              timer.seconds < 10 ? '0' + timer.seconds : timer.seconds
            }}
          </div>
        </div>
        <my-button v-if="!isStarted" @click.stop="setTopic">Generate Random Topic</my-button>
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
      fetchedTopics: [],
      currentTopics: [],
      currentTopic: 'click 👇 to start ',
      topicsAmount: 0,
      isStarted: false,
      isTopicSelected: false,
    };
  },
  mounted() {
    this.fetchTopics();
  },
  methods: {
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
    async fetchTopics() {
      try {
        const response = await axios.get('https://62a0f78a7b9345bcbe4358a7.mockapi.io/questions');
        let obj = {};

        for (let key in response.data[0]) {
          let elem = response.data[0][key];
          obj[key] = elem;
        }

        this.fetchedTopics = obj;
        for (let key in this.fetchedTopics) {
          this.fetchedTopics[key].forEach(el => {
            this.currentTopics.push(el);
          });
        }
      } catch (e) {
        console.log(e);
      }
    },
    setTopic() {
      if (!this.currentTopics.length) {
        this.currentTopic = 'No more topics available.';
        this.stopTimer();
      } else {
        let rnd = Math.floor(Math.random() * this.currentTopics.length);
        this.currentTopic = this.currentTopics[rnd];
        this.currentTopics.splice(rnd, 1);
        this.stopTimer();
        this.startTimer();
        this.topicsAmount++;
      }
    },
    changeFilter(topic) {
      if (!topic) {
        this.currentTopics = [];
        for (let key in this.fetchedTopics) {
          this.fetchedTopics[key].forEach(el => {
            this.currentTopics.push(el);
          });
        }
        this.currentTopic = 'ALL the topics';
        this.isTopicSelected = false;
      } else {
        this.currentTopics = this.fetchedTopics[topic];
        this.currentTopic = [topic] + ' topics';
        this.isTopicSelected = true;
      }
      this.topicsAmount = 0;
      this.stopTimer();
    },
    workaround() {
      console.log('workaround works');
    },
  },
};
</script>

<style lang="scss" scoped>
* {
  color: #e3ce0f;
}

.main-container {
  padding-top: 5vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.pop-up-block {
  width: 100vw;
}

.main {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 700px;

  & > div:not(:first-of-type) {
    margin: 30px 0;
    font-size: 3em;
  }

  h1 {
    margin: 30px 0;
  }

  .current-topic {
    width: 590px;
    height: 100px;
    padding: 80px 5px;
    border-radius: 10px;
    background-color: black;

    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
    position: relative;

    & > div {
      position: absolute;
      font-size: 1.5rem;
      opacity: 0.8;
    }

    .topics-amount {
      top: 5px;
      right: 10px;
    }

    .timer {
      bottom: 5px;
      left: 10px;
    }
  }
}

@media (min-width: 1500px) {
  .main-container {
    padding-top: calc(100vh / 5);
  }
}

@media (min-width: 1000px) {
  .main-container {
    padding-top: 10vh;
  }
}

@media (max-width: 600px) {
  .main-container {
    padding-top: 5vh;
  }

  .main {
    width: 414px;
    text-align: center;

    .current-topic {
      width: 400px;
    }
  }
}
</style>
