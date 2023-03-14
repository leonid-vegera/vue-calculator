<template>
  <div class="calculator">
    <div class="calculator__screen">
      <input
        type="text"
        class="calculator__input"
        v-model="result"
        placeholder="0"
        @keyup.enter="operationClickHandler('=')"
        ref="screen"
      />
    </div>

    <div class="calculator__panel">
      <div class="calculator__upper-panel">
        <div
          v-for="(operation, index) in operations"
          :key="index"
          @click="operationClickHandler(operation)"
          v-show="isUpperPanelOperation(operation)"
          class="calculator__operation"
          :class="`operation-${operation}`"
        >
          {{operation}}
        </div>
      </div>

      <div class="calculator__lower-panel">
        <div class="calculator__numbers">
          <div
              class="calculator__number"
              :class="`number-${number}`"
              v-for="(number, index) in values"
              :key="index"
              @click="numberClickHandler(number)"
          >
            {{number}}
          </div>
        </div>
        <div class="calculator__operations">
          <div
            class="calculator__operation"
            :class="`operation-${operation}`"
            v-for="(operation, index) in operations"
            :key="index"
            @click="operationClickHandler(operation)"
            v-show="isSidePanelOperation(operation)"
          >
            {{operation}}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CalculatorBody",
  data() {
    return {
      result: '',
      values: [1, 2, 3, 4, 5, 6, 7, 8, 9, '.', 0],
      operations: ['-', '*', '/', '=', 'C', '<', '%', '+'],
      chosenOperation: '',
    }
  },
  methods: {
    numberClickHandler(value) {
      if (this.values.includes(value)) {
        if (typeof this.result === 'string' && this.result.startsWith('0')) {
          this.result = this.result.replace('0', '')
        }
        if (value === '.' && this.result.includes('.')) {
          return;
        }
        this.result = this.result + value;
        this.$refs.screen.focus();
      }
    },
    operationClickHandler(value) {
      function percentHandler() {
        const operators = '+-/*';
        const firstIndexOfOper = this.result.split('').findIndex(item => operators.includes(item));
        const prevValue = +(this.result.slice(0, firstIndexOfOper));

        let lastIndexOfOper = this.result.split('').reverse().findIndex(char => operators.includes(char));
        if (lastIndexOfOper >= 0) {
          lastIndexOfOper = this.result.length - lastIndexOfOper - 1;
        }
        this.chosenOperation = this.result.split('').find((item, index) => +index === +lastIndexOfOper);
        const percentValue = this.result.slice(lastIndexOfOper + 1);
        let percentNumber = 0;
        if (this.chosenOperation === '+' || this.chosenOperation === '-') {
          percentNumber = prevValue * parseInt(percentValue) / 100;
        }
        if (this.chosenOperation === '*' || this.chosenOperation === '/') {
          percentNumber = parseInt(percentValue) / 100
        }
        this.result = new Function('return ' + prevValue + this.chosenOperation + percentNumber)();
      }

      switch (value) {
        case 'C':
          this.result = '';
          break;
        case '<':
          this.result = this.result.slice(0, -1);
          break;
        case '=':
          if (this.result.includes('%')) {
            percentHandler.call(this);
            break;
          }
          this.result = new Function('return ' + this.result)();
          break;
        default:
          this.result = this.result + value;
      }
    },
    isUpperPanelOperation(operation) {
      const upperOperations = ['C', '<', '%', '+'];
      return upperOperations.includes(operation);
    },
    isSidePanelOperation(operation) {
      const sideOperations = ['-', '*', '/', '='];
      return sideOperations.includes(operation);
    },
  },
}
</script>

<style scoped lang="scss">
  $gap: 2px;

  .calculator {
    margin: auto;
    background-color: #3A4655;
    max-width: 500px;
    width: 30%;
    min-width: 300px;
    padding: 20px 10px;

    &__screen {
      height: 20%;
      margin-bottom: 15px;
    }

    &__input {
      padding: 0 15px;
      height: 100%;
      width: 100%;
      outline: none;
      background-color: #3A4655;
      color: #f2f2f2;
      font-size: 2em;
      text-align: end;
      border: 1px solid transparent;
      box-sizing: border-box;

      &::placeholder {
        color: #f2f2f2;
      }
    }

    &__upper-panel {
      display: flex;
      justify-content: space-evenly;
      margin-bottom: $gap;
    }

    &__lower-panel {
      display: flex;
      gap: $gap;
    }

    &__panel {
      display: flex;
      flex-direction: column;
      width: 100%;
    }

    &__numbers {
      flex: 0 1 75%;
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: $gap;
    }

    &__operations {
      flex: 0 1 25%;
      display: flex;
      flex-direction: column;
      gap: $gap;
    }

    &__number,
    &__operation {
      display: flex;
      flex: 1 0 auto;
      justify-content: center;
      align-items: center;
      padding: 10px;
      background-color: #515b67;
      color: #f8f8f8;

      &.number-0 {
        grid-column: 2 span;
      }

      &:hover {
        background-color: #47505b;
      }

      &:active {
        background-color: #363d44;
        color: #b9b9b9;
      }
    }
  }
</style>
