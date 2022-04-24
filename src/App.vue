<template>
<div class="wrapper">
  <div class="counter"><span class="right">{{ counterRight }}</span> / <span class="wrong">{{ counterWrong }}</span></div>
  <div class="number">{{ number }}</div>
  <div class="serbian" :class="{ visible: isSerbianVisible }">{{ serbianDigit }}</div>
  <div class="buttons">
    <div class="button wrong" @click="clickForget">Не помню</div>
    <div class="button show-answer" @click="showAnswer">Покажи ответ</div>
    <div class="button right" @click="clickRemember">Помню</div>
  </div>
</div>
</template>

<script>
  const uniqueDigits = {
    0: "nula",
    1: "jedan",
    2: "dva",
    3: "tri",
    4: "četiri",
    5: "pet",
    6: "šest",
    7: "sedam",
    8: "osam",
    9: "devet",
    10: "deset",
    11: "jedanaest",
    14: "četrnaest",
    16: "šesnaest",
    40: "četrdeset",
    60: "šezdeset",
    90: "devedeset",
    100: "sto",
    200: "dvesta",
    300: "trista"
  };  

  export default {
    data() {
      return {
        counterRight: 0,
        counterWrong: 0,
        isSerbianVisible: false,
        number: 42
      }
    },
    computed: {
      serbianDigit() {
        return this.getSerbianDigit(this.number);
      }
    },
    methods: {
      clickRemember() {
        this.counterRight += 1;
        this.generateNew();
      },

      clickForget() {
        this.counterWrong += 1;
        this.generateNew();
      },

      generateNew() {
        this.hideAnswer();
        this.number = this.getRandomNumber();
      },

      hideAnswer() {
        this.isSerbianVisible = false;
      },

      showAnswer() {
        this.isSerbianVisible = true;
      },

      getRandomNumber() {
        return Math.floor(Math.random() * 999);
      },

      getFirstDigit(number) {
        return Number(String(number)[0]);
      },

      getSerbianUnder20(number) {
        switch (number) {
          case 11:
          case 14:
          case 16:
            return uniqueDigits[number];
          default:
            return  uniqueDigits[number % 10] + "naest";
        }
      },

      getSerbianUnder100(number) {
        const [first, second] = String(number).split("").map(Number);
        let processedSecond = second === 0 ? "" : uniqueDigits[second];
        switch (first) {
          case 4:
          case 6:
          case 9:
            return `${uniqueDigits[first * 10]} ${processedSecond}`;
          default:
            return `${uniqueDigits[first]}deset ${processedSecond}`;
        }
      },

      getSerbianUnder1000(number) {
        let delimiter = number % 100;
        let order = number - delimiter; 
        switch (order) {
          case 100:
          case 200:
          case 300:
            return `${uniqueDigits[order]} ${this.getSerbianDigit(delimiter)}`;
          default:
            return `${uniqueDigits[order / 100]}sto ${this.getSerbianDigit(delimiter)}`;
        }
      },

      getSerbianDigit(number) {
        if (number <= 10) {
          return uniqueDigits[number];
        } else if (number < 20) {
          return this.getSerbianUnder20(number);
        } else if (number < 100) {
          return this.getSerbianUnder100(number);
        } else if (number < 1000) {
          return this.getSerbianUnder1000(number);
        }
      }
    }
  }
</script>


<style>
@import '../assets/reset.css';

  html {
    overflow: hidden;
    font-family: 'Fira Sans', sans-serif;
    --wrong: #DD2C00;
    --right: #00C853;
    --normal: #3D5AFE;
  }
  #app {
    display: flex;
    justify-content: center;
    height: 90vh; 
    width: 100vw;
    overflow: hidden;
    padding: 40px 20px;
    box-sizing: border-box;
  }
  .wrapper {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
    height: 100%;
    max-width: 600px;
  }
  .counter {
    font-size: 12px;
  }
  .counter .right {
    color: var(--right);
  }
  .counter .wrong {
    color: var(--wrong);
  }
  .number {
    font-size: 90px;
  }
  .serbian {
    visibility: hidden;
    font-size: 20px;
  }
  .visible {
    visibility: visible;
  }
  .buttons {
    display: flex;
    flex-direction: row;
    gap: 10px;
    width: 100%;
  }
  .button {
    display: inline-flex;
    justify-content: center;
    align-items: center;
    flex-grow: 1;
    flex-basis: 33%;
    height: 40px;
    padding: 5px;
    cursor: pointer;
    border-radius: 5px;
    text-align: center;
    color: #fff;
  }
  .button.wrong {
    background-color: var(--wrong);
  }
  .button.show-answer {
    background-color: var(--normal);
  }
  .button.right {
    background-color: var(--right);
  }
</style>
