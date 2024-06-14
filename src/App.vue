<template>
  <div id="app">
    <h1>GreenAI Cost and CO2 Calculator</h1>
    <div class="form-group">
      <label for="gpu">GPU:</label>
      <select v-model="gpu" class="form-control">
        <option v-for="gpuOption in gpuOptions" :key="gpuOption" :value="gpuOption">{{ gpuOption }}</option>
      </select>
    </div>
    <div class="form-group">
      <label for="region">Region:</label>
      <select v-model="region" class="form-control">
        <option v-for="regionOption in regionOptions" :key="regionOption" :value="regionOption">{{ regionOption }}</option>
      </select>
    </div>
    <div class="form-group">
      <label for="provider">Provider:</label>
      <select v-model="provider" class="form-control">
        <option v-for="providerOption in providerOptions" :key="providerOption" :value="providerOption">{{ providerOption }}</option>
      </select>
    </div>
    <div class="form-group">
      <label for="time">Training Time (hours):</label>
      <input type="number" v-model.number="time" min="1" class="form-control" />
    </div>
    <div>
      <button @click="calculate" class="btn btn-primary">Calculate</button>
    </div>
    <div v-if="result" class="results">
      <h2>Results</h2>
      <p><strong>Cost:</strong> {{ result.cost }} USD</p>
      <p><strong>CO2 Emissions:</strong> {{ result.co2 }} kg</p>
      <p><strong>Energy Consumption:</strong> {{ result.energy }} kWh</p>
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
      const costPerHour = this.getCostPerHour();
      const co2PerHour = this.getCO2PerHour();
      const energyPerHour = this.getEnergyPerHour();

      const cost = (this.time * costPerHour).toFixed(2);
      const co2 = (this.time * co2PerHour).toFixed(2);
      const energy = (this.time * energyPerHour).toFixed(2);

      this.result = { cost, co2, energy };
    },
    getCostPerHour() {
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
  margin-top: 40px;
  max-width: 600px;
  margin: 0 auto;
}

.form-group {
  margin-bottom: 20px;
}

.form-control {
  width: 100%;
  padding: 10px;
  font-size: 1em;
}

.btn-primary {
  padding: 10px 20px;
  font-size: 1em;
  background-color: #007bff;
  border: none;
  color: white;
  cursor: pointer;
}

.btn-primary:hover {
  background-color: #0056b3;
}

.results {
  margin-top: 20px;
  text-align: left;
}

.results p {
  margin: 5px 0;
}
</style>
