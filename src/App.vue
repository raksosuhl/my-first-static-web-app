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
    async calculate() {
      // Beispielhafte API-Aufrufe und Berechnungen
      const cost = await this.getCost();
      const co2 = await this.getCO2Emissions();
      const energy = await this.getEnergyConsumption();

      this.result = { cost, co2, energy };
    },
    async getCost() {
      // Hier würden API-Aufrufe zu den Preis-APIs von AWS, Google Cloud und Azure erfolgen
      // Rückgabe eines Beispielwerts
      return (this.time * 0.5).toFixed(2); // Beispielrechnung
    },
    async getCO2Emissions() {
      // Hier würden API-Aufrufe zur Berechnung der CO2-Emissionen erfolgen
      // Rückgabe eines Beispielwerts
      return (this.time * 0.3).toFixed(2); // Beispielrechnung
    },
    async getEnergyConsumption() {
      // Hier würden API-Aufrufe zur Berechnung des Energieverbrauchs erfolgen
      // Rückgabe eines Beispielwerts
      return (this.time * 2).toFixed(2); // Beispielrechnung
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
