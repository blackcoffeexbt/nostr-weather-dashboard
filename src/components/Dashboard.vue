<template>
  <div id="app">
    <h1>Nostr Mesh Weather Dashboard</h1>

  <form @submit.prevent="requestData">
    <fieldset>
      <label>Enter a weather node's pubkey (hex format):</label>
      <input v-model="weatherpubkey">
    </fieldset>
    <fieldset>
      <label>How many records to retrieve on first request?</label>
      <input v-model="limit">
    </fieldset>
    <input type="submit" value="Get weather data"/>
  </form>
    <br/>
    <!--    <button @click="sendMessage">Send Message</button>-->
    <line-chart class="weather-chart" :chart-data="tempChartData"></line-chart>
    <!-- <line-chart class="weather-chart" :chart-data="humidityChartData"></line-chart> -->
    <line-chart class="weather-chart" :chart-data="pressureChartData"></line-chart>
    <p>{{ message }}</p>
  </div>
</template>

<script>
import LineChart from "@/components/LineChart";
export default {
  name: "Dashboard",
  components: {LineChart},
  data() {
    return {
      socket: null,
      message: "",
      weatherpubkey: "d0bfc94bd4324f7df2a7601c4177209828047c4d3904d64009a3c67fb5d5e7ca",
      limit: 10,
      tempChartData: {
        labels: [],
        datasets: [
          {
            label: 'Temperature',
            data: [],
            borderColor: 'red',
          }
        ]
      },
      pressureChartData: {
        labels: [],
        datasets: [
          {
            label: 'Pressure',
            data: [],
            borderColor: 'green',
          }
        ]
      },
      humidityChartData: {
        labels: [],
        datasets: [
          {
            label: 'Humidity',
            data: [],
            borderColor: 'blue',
          },
        ]
      },
      tempDataArray: [],
      pressureDataArray: [],
      humidityDataArray: [],
    };
  },
  methods: {
    requestData() {
      // clear the data arrays as well
      this.tempDataArray = []
      this.pressureDataArray = []
      this.humidityDataArray = []

      this.pressureChartData.labels = []
      this.pressureChartData.datasets[0].data = []
      this.tempChartData.labels = []
      this.tempChartData.datasets[0].data = []
      this.humidityChartData.labels = []
      this.humidityChartData.datasets[0].data = []
      let request = `["REQ", "12312312lkj12lk3j1l", {"authors": ["${this.weatherpubkey}"], "kinds": [8003, 8004], "limit": ${this.limit}}]`
      console.log(request)
      this.socket.send(request);
    },
    setupSocket() {
      this.socket = new WebSocket('wss://nos.lol');
      this.socket.onopen = () => {
        console.log('Connected');
      };

      this.socket.onerror = error => {
        console.log(`WebSocket error: ${error}`);
      };

      this.socket.onmessage = event => {
        console.log('Message from server ', event.data);
        // this.message = event.data;
        if(event.data.indexOf("Temperature")) {
          this.parseMessage(JSON.parse(event.data));
        }
      };
    },
    formatDate(date) {
      const yyyy = date.getFullYear();
      const MM = String(date.getMonth() + 1).padStart(2, '0'); // Months are zero-based
      const dd = String(date.getDate()).padStart(2, '0');
      const HH = String(date.getHours()).padStart(2, '0');
      const mm = String(date.getMinutes()).padStart(2, '0');

      return `${yyyy}-${MM}-${dd} ${HH}:${mm}`;
    },
    parseMessage(message) {
      console.log(message);
      const content = message[2].content;
      const createdAt = this.formatDate(new Date(message[2].created_at * 1000)); // convert to milliseconds

      // if kind is 8003, then it's a temperature reading
      // if kind is 8004, then it's a pressure reading
      if(message[2].kind == 8003) {
        const temperature = parseFloat(content);
        this.tempDataArray.push({time: createdAt, value: temperature});
        this.tempDataArray.sort((a, b) => a.time.localeCompare(b.time));
        this.updateChartData(this.tempChartData, this.tempDataArray, 'Temperature', 'red');
      }
      else if(message[2].kind == 8004) {
        const pressure = parseFloat(content);
        this.pressureDataArray.push({time: createdAt, value: pressure});
        this.pressureDataArray.sort((a, b) => a.time.localeCompare(b.time));
        this.updateChartData(this.pressureChartData, this.pressureDataArray, 'Pressure', 'green');
      }
    },
    updateChartData(chartData, dataArray, label, color) {
      chartData.labels = dataArray.map(item => item.time);
      chartData.datasets = [
        {
          label: label,
          data: dataArray.map(item => item.value),
          borderColor: color,
        }
      ];
    }
  },
  mounted() {
    this.setupSocket();
  },
  beforeDestroy() {
    if (this.socket) {
      this.socket.close();
    }
  }
};
</script>

<style scoped>
#app {
  font-family: Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
}

input, button {
  padding: 5px;
}

input {
  width: 400px;
  max-width: 100%;
}

label {
  margin-right: 10px;
}

fieldset {
  border: 0;
}
</style>
