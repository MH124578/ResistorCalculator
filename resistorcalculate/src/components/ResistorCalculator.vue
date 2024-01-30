<template>
  <div class="resistor-calculator">
    <h1>Widerstandsrechner</h1>
    <div class="input-group">
    <input 
      v-model="newValue" 
      type="number" 
      placeholder="Wert eingeben" 
      class="input-field" 
      :class="{'input-invalid': isInputInvalid()}"
    >
    <button @click="addResistorValue">Hinzufügen</button>
  </div>
    <div class="resistor-values">
      <span v-for="(value, index) in resistorValues" :key="index" class="resistor-value">
        {{ value }}Ω
      </span>
    </div>
    <div class="calculation-type">
      <button @click="calculationType = 'series'" :class="{'active-button': calculationType === 'series'}">Serie</button>
      <button @click="calculationType = 'parallel'" :class="{'active-button': calculationType === 'parallel'}">Parallel</button>
    </div>
    <div v-if="isValidInput" class="result">
      Gesamtwiderstand: {{ result }}Ω
    </div>
    <button @click="clearValues" class="clear-button">Löschen</button>
  </div>
</template>

<script>
export default {
  name: 'ResistorCalculator',
  data() {
    return {
      resistorValues: [],
      calculationType: 'series',
      newValue: ''
    };
  },
  computed: {
    result() {
      return this.calculationType === 'series'
        ? this.resistorValues.reduce((acc, val) => acc + val, 0)
        : 1 / this.resistorValues.reduce((acc, val) => acc + (1 / val), 0);
    },
    isValidInput() {
      return this.resistorValues.length > 0 && this.resistorValues.every(val => val > 0);
    }
  },
  methods: {
    addResistorValue() {
      const value = parseFloat(this.newValue);
      if (!isNaN(value) && value > 0) {
        this.resistorValues.push(value);
        this.newValue = '';
      }
    },
    clearValues() {
      this.resistorValues = [];
    },
    isInputInvalid() {
      if (this.newValue === '' || isNaN(this.newValue)) {
        return false;
      }
      const value = parseFloat(this.newValue);
      return value <= 0;
    }
  }
};
</script>

<style>
.resistor-calculator {
  max-width: 600px;
  margin: auto;
  padding: 20px;
  background-color: #f3f3f3;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.input-group {
  margin-bottom: 20px;
}

.input-field, .add-button, .clear-button, .active-button {
  padding: 10px;
  border-radius: 4px;
  border: 1px solid #ddd;
  margin-right: 10px;
}

.input-field {
  width: calc(70% - 20px);
}

.input-invalid {
  border: 2px solid red;
}

.add-button, .clear-button {
  width: 30%;
  background-color: #0055ff;
  color: white;
  cursor: pointer;
}

.add-button:hover, .clear-button:hover {
  background-color: #003399;
}

.active-button {
  background-color: #4CAF50;
  color: white;
}

.resistor-value {
  margin: 5px;
  display: inline-block;
  padding: 5px 10px;
  background-color: #eeeeee;
  border-radius: 4px;
}

.result {
  margin-top: 20px;
  font-size: 1.2em;
  color: #333333;
}
</style>
