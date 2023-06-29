<template>
  <div id="app">
    <h1>Nostr Weather Dashboard</h1>

    <fieldset>
      <label>Enter a weather node's pubkey (hex format):</label>
      <input v-model="weatherpubkey">
    </fieldset>
    <fieldset>
      <label>How many records to retrieve on first request?</label>
      <input v-model="limit">
    </fieldset>

    <button @click="requestData()">Get weather data</button>
    <br/>
    <!--    <button @click="sendMessage">Send Message</button>-->
    <line-chart class="weather-chart" :chart-data="tempChartData"></line-chart>
    <line-chart class="weather-chart" :chart-data="humidityChartData"></line-chart>
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
      }
    };
  },
  methods: {
    requestData() {
      this.pressureChartData.labels = []
      this.pressureChartData.datasets[0].data = []
      this.tempChartData.labels = []
      this.tempChartData.datasets[0].data = []
      this.humidityChartData.labels = []
      this.humidityChartData.datasets[0].data = []
      let request = `["REQ", "12312312lkj12lk3j1l", {"authors": ["${this.weatherpubkey}"], "kinds": [1], "limit": ${this.limit}}]`
      console.log(request)
      this.socket.send(request);
    },
    setupSocket() {
      this.socket = new WebSocket('wss://nostr.mom');
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
      const content = message[2].content;
      const createdAt = this.formatDate(new Date(message[2].created_at * 1000)); // convert to milliseconds

      const tempMatch = content.match(/Temperature is (\d+\.\d+)/);
      const humidityMatch = content.match(/Humidity is (\d+\.\d+)/);
      const pressureMatch = content.match(/Pressure is (\d+\.\d+)/);

      if (tempMatch && humidityMatch && pressureMatch) {
        const temperature = parseFloat(tempMatch[1]);
        const humidity = parseFloat(humidityMatch[1]);
        const pressure = parseFloat(pressureMatch[1]);

        this.tempChartData = {
          labels: [...this.tempChartData.labels, createdAt],
          datasets: [
            {
              label: 'Temperature',
              data: [...this.tempChartData.datasets[0].data, temperature],
              borderColor: 'red',
            }
          ]
        };

        this.humidityChartData = {
          labels: [...this.humidityChartData.labels, createdAt],
          datasets: [
            {
              label: 'Humidity',
              data: [...this.humidityChartData.datasets[0].data, humidity],
              borderColor: 'blue',
            }
          ]
        };

        this.pressureChartData = {
          labels: [...this.pressureChartData.labels, createdAt],
          datasets: [
            {
              label: 'Pressure',
              data: [...this.pressureChartData.datasets[0].data, pressure],
              borderColor: 'green',
            }
          ]
        };
      }
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
