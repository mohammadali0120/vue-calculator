<template>
  <div class="container">
    <div class="calc">
      <div class="calc__display">
        <p class="calc__display_input">
          {{ result || 0 }}
        </p>
        <div class="calc__display_divider"></div>
        <p class="calc__display_status">
          {{ actionList + current || 0 }}
        </p>
      </div>
      <div class="calc__content">
        <div class="calc__content_left">
          <ul class="calc__content_operator">
            <li class="calc__content_operator-item" @click="clear">AC</li>
            <li class="calc__content_operator-item" @click="clearOne">CE</li>
            <li class="calc__content_operator-item" @click="percent">%</li>
          </ul>
          <ul class="calc__content_numbers--top">
            <li
              v-for="item in items"
              :key="item.id"
              class="calc__content_numbers-item"
              @click="append(item.value)"
            >
              {{ item.id }}
            </li>
          </ul>
          <ul class="calc__content_numbers--bottom">
            <li class="calc__content_numbers-item" @click="append('0')">0</li>
            <li class="calc__content_numbers-item" @click="dot">.</li>
          </ul>
        </div>
        <div class="calc__content_right">
          <ul class="calc__content__operator">
            <li class="calc__content__operator-item" @click="add">+</li>
            <li class="calc__content__operator-item" @click="minus">-</li>
            <li class="calc__content__operator-item" @click="times">&times;</li>
            <li class="calc__content__operator-item" @click="divide">
              &divide;
            </li>
            <li class="calc__content__operator-item" @click="equl">=</li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      current: "",
      operatorClicked: false,
      result: "",
      actionList: "",
      operatorSign: "",
      items: [
        { id: 1, value: "1" },
        { id: 2, value: "2" },
        { id: 3, value: "3" },
        { id: 4, value: "4" },
        { id: 5, value: "5" },
        { id: 6, value: "6" },
        { id: 7, value: "7" },
        { id: 8, value: "8" },
        { id: 9, value: "9" },
      ],
    };
  },
  methods: {
    clear() {
      this.current = "";
      this.result = "";
      this.actionList = "";
      this.operatorClicked = false;
      this.operatorSign = "";
    },
    clearOne() {
      if (this.current.length) {
        this.current = this.current.substring(0, this.current.length - 1);
        this.append("");
      } else {
        this.actionList = this.actionList.substring(
          0,
          this.actionList.length - 1
        );
        this.append("");
      }
    },
    percent() {
      this.current = `${parseFloat(this.current) / 1000}`;
    },
    append(number) {
      if (this.operatorClicked) {
        this.current = "";
        this.operatorClicked = false;
      }
      this.current = `${this.current}${number}`;
    },
    addToAction(operator) {
      if (this.operatorClicked === false) {
        this.actionList += `${this.current}${operator}`;
        this.current = "";
        this.operatorClicked = true;
      }
    },
    dot() {
      if (this.current.includes(".") !== -1) {
        this.append(".");
      }
    },
    divide() {
      this.operatorSign = "/";
      this.addToAction("/");
    },
    times() {
      this.operatorSign = "*";
      this.addToAction("*");
    },
    add() {
      this.operatorSign = "+";
      this.addToAction("+");
    },
    minus() {
      this.operatorSign = "-";
      this.addToAction("-");
    },
    equl() {
      if (this.operatorClicked === false && isNaN(this.actionList.charAt(0))) {
        this.result = "NaN";
      } else if (this.current.length >= 1) {
        // eslint-disable-next-line no-eval
        this.result = eval(this.actionList + this.current);
      } else {
        this.result = "NaN";
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.container {
  width: 1300px;
  height: 100%;
  margin: 0 auto;
  background: #f1f1f1;
  .calc {
    width: 40%;
    margin: 0 auto;
    padding: 20px;
    .calc__display {
      padding: 0;
      width: 100%;
      height: 80px;
      &_input {
        height: 48px;
        width: 100%;
        background: #fff;
        border: none;
        padding: 12px 10px;
        border-radius: 7px 7px 0 0;
        border: 3px solid transparent;
        color: #000;
        font-weight: bold;
        font-size: 23px;
        &:focus {
          outline: 0;
        }
      }
      &_divider {
        width: 100%;
        height: 1px;
        background: #0000001a;
      }
      &_status {
        background: #fff;
        padding: 5px 10px;
        height: 33px;
        margin: 0;
        color: #000;
        font-weight: bold;
      }
    }
    .calc__content {
      display: flex;
      flex-wrap: wrap;
      height: 100%;
      width: 100%;
      .calc__content_left {
        width: 75%;
        flex-basis: 75%;
        background: green;
        // #EDECFE
        border-radius: 0 0 0 7px;
        .calc__content_operator {
          list-style: none;
          display: flex;
          flex-wrap: wrap;
          padding: 0;
          .calc__content_operator-item {
            background: #f0e0fc;
            flex-basis: 33.33%;
            text-align: center;
            font-size: 25px;
            font-weight: bold;
            cursor: pointer;
            border-bottom: 1px solid;
            padding: 15px 0;
            border: 1px solid #0000001a;
          }
        }
        .calc__content_numbers--top {
          list-style: none;
          display: flex;
          flex-wrap: wrap;
          padding: 0;
          .calc__content_numbers-item {
            background: #edecfe;
            flex-basis: 33.33%;
            text-align: center;
            font-size: 25px;
            font-weight: bold;
            cursor: pointer;
            border-bottom: 1px solid;
            padding: 15px 0;
            border: 1px solid #0000001a;
          }
        }
        .calc__content_numbers--bottom {
          list-style: none;
          display: flex;
          flex-wrap: wrap;
          padding: 0;
          .calc__content_numbers-item:first-child {
            flex-basis: 66.66%;
          }
          .calc__content_numbers-item:last-child {
            flex-basis: 33.33%;
          }
          .calc__content_numbers-item {
            background: #edecfe;
            text-align: center;
            font-size: 25px;
            font-weight: bold;
            cursor: pointer;
            border-bottom: 1px solid;
            padding: 15px 0;
            border: 1px solid #0000001a;
          }
        }
      }
      .calc__content_right {
        width: 25%;
        flex-basis: 25%;
        .calc__content__operator {
          list-style: none;
          display: flex;
          flex-wrap: wrap;
          // height: 100%;
          .calc__content__operator-item {
            flex-basis: 100%;
            background: #ffc46c;
            text-align: center;
            font-size: 25px;
            font-weight: bold;
            cursor: pointer;
            padding: 15px 0;
            border: 1px solid #0000001a;
          }
          .calc__content__operator-item:last-child {
            border-radius: 0 0 7px 0;
            border-bottom: 0;
          }
        }
      }
    }
  }
}
</style>
<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
</style>
