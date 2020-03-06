<template>
  <div class="container d-col justify-content-center vertical-center">
    <h2 class="title">Typing Test</h2>
    <hr />
    <div class="row">
      <input class="typing-area" type="text" v-model="userInput" />
    </div>
    <br />
    <div class="text">
      <span
        :style="styleCurrentWord(index)"
        v-for="(word, index) in textArray"
        :key="index"
        >{{ word }}
      </span>
    </div>
    <hr />
    <div class="timer">{{ startedTyping }}</div>
    <div v-show="neverShow">{{ isWordCorrect }}</div>
    <div class="speed" v-show="userStartedTyping">
      Num Correct: {{ correctAmount }} WPM: {{ wpm }}
    </div>
  </div>
</template>

<script>
let testText =
  "Word processors evolved dramatically once they became software programs rather than dedicated machines. They can usefully be distinguished from text editors, the category of software they evolved from. Word processing added to the text editor the ability to control type style and size, to manage lines (word wrap), to format documents into pages, and to number pages. Functions now taken for granted were added incrementally, sometimes by purchase of independent providers of add-on programs. Spell checking, grammar checking and mail merge were some of the most popular add-ons for early word processors. Word processors are also capable of hyphenation, and the management and correct positioning of footnotes and endnotes. Later desktop publishing programs were specifically designed with elaborate pre-formatted layouts for publication, offering only limited options for changing the layout, while allowing users to import text that was written using a text editor or word processor, or type the text in themselves.";

export default {
  preserveWhiteSpace: true,
  data() {
    return {
      correctAmount: 0,
      step: 0,
      time: 60,
      userStartedTyping: false,
      userInput: "",
      text: testText,
      textArray: testText.split(" "),
      currentWord: "",
      neverShow: false,
      wpm: 0,
      numOfCharsTyped: testText.split("").length
    };
  },
  methods: {
    getWPM() {
      if (this.time === 0) {
        return;
      }
      this.wpm = this.numOfChars / 5 / 1;
    },
    styleCurrentWord(index) {
      let style = {};

      if (index === this.step) {
        style.color = "red";
      }

      return style;
    },
    clearInput() {
      this.userInput = "";
    },
    increaseStep() {
      this.step = this.step + 1;
      this.correctAmount = this.correctAmount + 1;
    },
    getCurrentWord() {
      this.currentWord = this.textArray[this.step];

      console.log(this.currentWord);
    },
    checkInputLen() {
      if (this.userInput.length > 0) {
        this.userStartedTyping = true;
      }
    },
    timer() {
      if (this.time === 0) {
        return;
      }
      let timer = setTimeout(() => {
        if (this.time === 0) {
          clearTimeout(timer);
        }
        this.time--;
      }, 1000);
    }
  },
  computed: {
    startedTyping() {
      if (this.userStartedTyping) {
        this.timer();
        //logic to manipulate text here
      }
      return this.time;
    },
    isWordCorrect() {
      console.log(this.userInput, this.textArray[this.step]);
      if (this.userInput === this.textArray[this.step]) {
        this.increaseStep();
        this.clearInput();
        return true;
      } else {
        return false;
      }
    }
  },
  updated() {
    this.checkInputLen();
    this.getWPM();
  },
  mounted() {
    this.getCurrentWord();
  }
};
</script>

<style>
.vertical-center {
  min-height: 100%;
  min-height: 100vh;
  align-items: center;
}

.title,
.speed {
  text-align: center;
}

.typing-area {
  width: 500px;
  margin: 0 auto;
}

.text {
  margin: 0 auto;
  text-align: justify;
  max-width: 500px;
}

.timer {
  margin: 0 auto;
  text-align: center;
}
</style>
