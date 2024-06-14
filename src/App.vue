<template>
  <div id="app">
    <h1>GreenAI Cost and CO2 Calculator</h1>
    <div class="input-group">
      <label for="query">Query:</label>
      <input type="text" v-model="query" class="form-control" placeholder="Enter your query" />
    </div>
    <div class="info-box">
      <p>Include the following in your query:</p>
      <ul>
        <li>What is your budget?</li>
        <li>Where should the servers be located?</li>
        <li>What type of AI training should be performed?</li>
        <li>Do you know which model you want to use?</li>
      </ul>
    </div>
    <div>
      <button @click="calculate" class="btn btn-primary">Calculate</button>
    </div>
    <div v-if="result" class="results">
      <h2>Results</h2>
      <p><strong>Cost:</strong> {{ result.cost }} USD</p>
      <p><strong>CO2 Emissions:</strong> {{ result.co2 }} kg</p>
      <p><strong>Energy Consumption:</strong> {{ result.energy }} kWh</p>
      <div class="rating">
        <p>Rating:</p>
        <span v-for="star in 5" :key="star" class="star" :class="{ filled: star <= result.rating }">★</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      query: "",
      result: null
    };
  },
  methods: {
    calculate() {
      // Beispielhafte Berechnungen basierend auf festen Werten
      const cost = (Math.random() * 1000).toFixed(2);
      const co2 = (Math.random() * 500).toFixed(2);
      const energy = (Math.random() * 300).toFixed(2);
      const rating = Math.floor(Math.random() * 5) + 1;

      this.result = { cost, co2, energy, rating };
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

.input-group {
  margin-bottom: 20px;
}

.form-control {
  width: 100%;
  padding: 10px;
  font-size: 1em;
}

.info-box {
  background-color: #f9f9f9;
  border: 1px solid #ddd;
  padding: 10px;
  margin-bottom: 20px;
  text-align: left;
}

.info-box ul {
  list-style-type: disc;
  margin: 10px 0 0 20px;
  padding: 0;
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

.rating {
  margin-top: 10px;
}

.star {
  font-size: 2em;
  color: #ddd;
}

.star.filled {
  color: gold;
}
</style>
