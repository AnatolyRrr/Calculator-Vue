<template>
  <div class="container">
    <div class="calculator">
      <div class="display__wrap">
        <div class="formula__wrap">
          <p class="formula">{{ formula }}</p>
        </div>
        <input
          class="input"
          v-model="result"
          placeholder="0"
          @keydown.esc="clear()"
        />
        <div class="btns__top">
          <div
            class="btns btns__op"
            v-for="n in operationsTop"
            :key="n"
            @click="action(n)"
          >
            {{ n }}
          </div>
        </div>
        <div class="btns__bot-wrap">
          <div class="btns__bot-left">
            <div
              class="btns btns__n"
              v-for="n in numbers"
              :key="n"
              @click="action(n)"
            >
              {{ n }}
            </div>
          </div>
          <div class="btns__bot-right">
            <div
              class="btns btns__op"
              v-for="n in operationsBottom"
              :key="n"
              @click="action(n)"
            >
              {{ n }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      result: "",
      formula: "",
      numbers: [7, 8, 9, 4, 5, 6, 1, 2, 3, "00", 0, "."],
      operationsTop: ["C", "√", "%", "/"],
      operationsBottom: ["*", "-", "+", "="],
      operator: "",
      previousValue: 0,
    };
  },

  methods: {
    clear() {
      this.result = "";
      this.formula = "";
      this.operator = "";
      this.previousValue = 0;
    },

    action(n) {
      if (n === "C") {
        this.clear();
      }

      if (!isNaN(n) || n === ".") {
        this.result += n;
      }

      if (n === "%") {
        this.result = this.result / 100;
      }

      if (n === "√") {
        this.result = Math.sqrt(this.result);
      }

      if (["/", "*", "-", "+"].includes(n)) {
        if (
          this.result.length > 0 &&
          this.formula.length === 0 &&
          this.result !== "-"
        ) {
          this.previousValue = this.result;
          this.operator = n;
          this.formula = this.result + " " + n;
          this.result = "";
        } else if (this.result.length > 0 && this.formula.length > 0) {
          if (this.operator === "/") {
            this.formula = this.formula + " " + this.result + " " + n;
            this.previousValue =
              parseFloat(this.previousValue) / parseFloat(this.result);
            this.operator = n;
            this.result = "";
          }

          if (this.operator === "*") {
            this.formula = this.formula + " " + this.result + " " + n;
            this.previousValue =
              parseFloat(this.previousValue) * parseFloat(this.result);
            this.operator = n;
            this.result = "";
          }

          if (this.operator === "-") {
            this.formula = this.formula + " " + this.result + " " + n;
            this.previousValue =
              parseFloat(this.previousValue) - parseFloat(this.result);
            this.operator = n;
            this.result = "";
          }

          if (this.operator === "+") {
            this.formula = this.formula + " " + this.result;
            this.previousValue =
              parseFloat(this.previousValue) + parseFloat(this.result);
            this.operator = n;
            this.result = "";
          }
        } else if (n === "-" && this.result.length === 0) {
          this.result += n;
        } else if (this.result.length === 0 && this.formula.length > 0) {
          this.operator = "";
        }
      }

      if (n === "=" && this.result.length > 0) {
        this.formula = this.formula + " " + this.result + " " + "=";

        if (this.operator === "/") {
          this.result =
            parseFloat(this.previousValue) / parseFloat(this.result);
        }

        if (this.operator === "*") {
          this.result =
            parseFloat(this.previousValue) * parseFloat(this.result);
        }

        if (this.operator === "-") {
          this.result =
            parseFloat(this.previousValue) - parseFloat(this.result);
        }

        if (this.operator === "+") {
          this.result =
            parseFloat(this.previousValue) + parseFloat(this.result);
        }

        this.operator = "";
        this.previousValue = 0;
      }

      if (n === "=" && this.result.length === 0) {
        this.formula = this.formula.split(" ").slice(0, -1).join(" ") + "=";
        this.operator = "";
        this.result = this.previousValue;
      }
    },
  },
};
</script>

<style>
.container {
  margin: 0 auto;
  margin-top: 34px;
  padding: 38px 31px;
  max-width: 616px;
  height: auto;
  background-color: rgb(255, 255, 255, 0.2);
  border-radius: 18px;
}

.calculator {
  padding: 16px 46px;
  color: #f2f2f2;
  max-width: 554px;
  height: auto;
  background: linear-gradient(155.23deg, #28518e 0%, #3a77d1 100%);
  border-radius: 18px;
  box-shadow: 0px 82px 158px rgba(0, 0, 0, 0.35),
    0px 24.7206px 47.6324px rgba(0, 0, 0, 0.228056),
    0px 10.2677px 19.7841px rgba(0, 0, 0, 0.175),
    0px 3.71362px 7.1555px rgba(0, 0, 0, 0.121944);
}

.display__wrap {
  max-width: 460px;
  margin: 0 auto;
}

.formula__wrap {
  height: 117px;
  display: flex;
  align-items: flex-end;
  justify-content: end;
}

.formula {
  font-size: 24px;
  line-height: 32px;
}

.input {
  font-size: 56px;
  line-height: 80px;
  color: #f2f2f2;
  width: 100%;
  text-align: right;
  margin-top: 28px;
  padding-bottom: 18px;
  border: 0;
  border-bottom: 2px solid rgb(255, 255, 255, 0.5);
  outline: none;
  background-color: rgb(255, 255, 255, 0);
}

.btns {
  width: 80px;
  height: 80px;
  padding: 0;
  margin: 0px 10px 16px;
  background-color: rgb(255, 255, 255, 0);
  border: 0;
  border-radius: 50%;
  color: #f2f2f2;
  font-size: 36px;
  line-height: 80px;
  text-align: center;
  transition: all ease-in 0.2s;
}

.btns__n:hover {
  background: rgb(255, 255, 255, 0.12);
}

.btns__op:hover {
  color: #2b589a;
  background: rgb(255, 255, 255, 0.8);
}

.btns__bot-wrap {
  display: flex;
  justify-content: space-between;
}

.btns__top {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  margin-top: 37px;
}

.btns__bot-left {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  padding-right: 6px;
}

.btns__bot-right {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-end;
}
</style>
