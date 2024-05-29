<template>
  <div id="app">
    <h1>GreenAI Cost and CO2 Calculator</h1>
    <div>
      <label for="gpu">GPU:</label>
      <select v-model="gpu">
        <option v-for="gpuOption in gpuOptions" :key="gpuOption" :value="gpuOption">{{ gpuOption }}</option>
      </select>
    </div>
    <div>
      <label for="region">Region:</label>
      <select v-model="region">
        <option v-for="regionOption in regionOptions" :key="regionOption" :value="regionOption">{{ regionOption }}</option>
      </select>
    </div>
    <div>
      <label for="provider">Provider:</label>
      <select v-model="provider">
        <option v-for="providerOption in providerOptions" :key="providerOption" :value="providerOption">{{ providerOption }}</option>
      </select>
    </div>
    <div>
      <label for="time">Training Time (hours):</label>
      <input type="number" v-model.number="time" />
    </div>
    <div>
      <button @click="calculate">Calculate</button>
    </div>
    <div v-if="result">
      <h2>Results</h2>
      <p>Cost: {{ result.cost }} USD</p>
      <p>CO2 Emissions: {{ result.co2 }} kg</p>
      <p>Energy Consumption: {{ result.energy }} kWh</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      gpu: "NVIDIA Tesla V100",
      region: "Frankfurt, Germany",
      provider: "AWS",
      time: 1,
      result: null,
      gpuOptions: ["NVIDIA Tesla V100", "NVIDIA Tesla P100", "NVIDIA Tesla T4"],
      regionOptions: ["Frankfurt, Germany", "Boston, USA", "Shenzhen, China", "Bangalore, India"],
      providerOptions: ["AWS", "Google Cloud", "Azure"]
    };
  },
  methods: {
    calculate() {
      // Beispielhafte Berechnungen basierend auf festen Werten
      const costPerHour = this.getCostPerHour();
      const co2PerHour = this.getCO2PerHour();
      const energyPerHour = this.getEnergyPerHour();

      const cost = (this.time * costPerHour).toFixed(2);
      const co2 = (this.time * co2PerHour).toFixed(2);
      const energy = (this.time * energyPerHour).toFixed(2);

      this.result = { cost, co2, energy };
    },
    getCostPerHour() {
      // Feste Kosten pro Stunde basierend auf GPU und Anbieter
      const baseCost = {
        "NVIDIA Tesla V100": 3.0,
        "NVIDIA Tesla P100": 2.5,
        "NVIDIA Tesla T4": 2.0
      };
      const providerMultiplier = {
        "AWS": 1.0,
        "Google Cloud": 1.1,
        "Azure": 1.2
      };
      return baseCost[this.gpu] * providerMultiplier[this.provider];
    },
    getCO2PerHour() {
      // Feste CO2-Emissionen pro Stunde basierend auf GPU und Region
      const baseCO2 = {
        "NVIDIA Tesla V100": 1.5,
        "NVIDIA Tesla P100": 1.2,
        "NVIDIA Tesla T4": 1.0
      };
      const regionMultiplier = {
        "Frankfurt, Germany": 0.8,
        "Boston, USA": 1.0,
        "Shenzhen, China": 1.5,
        "Bangalore, India": 1.2
      };
      return baseCO2[this.gpu] * regionMultiplier[this.region];
    },
    getEnergyPerHour() {
      // Fester Energieverbrauch pro Stunde basierend auf GPU
      const baseEnergy = {
        "NVIDIA Tesla V100": 0.9,
        "NVIDIA Tesla P100": 0.7,
        "NVIDIA Tesla T4": 0.5
      };
      return baseEnergy[this.gpu];
    }
  }
};
</script>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
