<template>
  <div class="container">
    <div class="about">
    <h1>What is this thing??</h1>
    <p>This is a simple dashboard that displays data from environmental sensors broadcasting data to nostr relays.</p>
    <p>This is a proof-of-concept for <a href="https://github.com/nostr-protocol/nips/blob/iot/91.md" target="_blank">Nostr Implementation Possibility 91</a>.</p>
    <p>Created by <a href="https://iris.to/npub1dqepr0g4t3ahvnjtnxazvws4rkqjpxl854n29wcew8wph0fmw90qlsmmgt">BC</a></p>
    <p><a href="https://github.com/blackcoffeexbt/nostr-weather-dashboard">Project GitHub Repo</a></p>
    <!-- <h2>Huh?</h2>
    <p>The mesh network is a self-healing esp32 mesh network with one or many internet gateway nodes (IGNs). The IGNs broadcast to the mesh that they
      can connect to the internet. The non-IGN nodes, route nostr events to the IGNs. The IGNs then broadcast the events to the nostr relays.</p>
    <h2>Moar please</h2>
    <h3>Internet Gateway Nodes (IGNs)</h3>
    <p>The IGNs consist of two ESP32 microcontrollers which talk to each other using a Serial connection.</p>
    <p>One of the microcontrollers is connected to the internet and is responsible for broadcasting the events to the nostr relays as well as passing a heartbeat timestamp to the microcontroller connected to it.</p>
    <p>The other microcontroller is connected to the mesh network and is responsible for receiving events from the mesh network and passing them to the internet connected microcontroller connected to it.</p>
    <h3>Mesh Network Nodes</h3>
    <p>Other nodes in the mesh network receive heartbeat events from the IGNs and pass them along to other nodes in the mesh network. They also receive events from other nodes in the mesh network and pass them along to the IGNs. These nodes don't have to broadcast environmental data, they could be broadcasting any data to nostr relays via the mesh.</p>
    <p>These nodes are ESP32 microcontrollers with a BME280 sensor attached. The nodes gather data from these sensors, construct nostr events and pass them to an IGN for broadcast.</p>
    -->
    <h3>Ok, what's the stack?</h3>
    <h4>Hardware</h4>
    <ul>
        <li>Some ESP32s</li>
        <li>BME280 and BMP280 sensors</li>  
    </ul>
    <h4>Firmware</h4>
    <ul>
      <li>ESP32 firmware written using platformio</li>
        <li>Arduino framework</li>
        <li><a href="https://registry.platformio.org/libraries/lnbits/Nostr">nostr-arduino</a></li>
        <li>Loads of other libs that aren't that significant</li>
    </ul>
    <h4>Web app dashboard</h4>
    <ul>
      <li>A Vue.js web app (<a href="https://www.forbes.com/sites/antoniopequenoiv/2023/06/26/jack-dorsey-backed-damus-removed-from-app-store-over-bitcoin-tipping-dispute/">It's also a PWA!</a>)</li>
      <li>chart.js to make things pretty</li>
      </ul>
    <!-- <h3>Is the project open source?</h3>
    <p>No, but it will be. I just haven't removed some doxable stuff and cleaned it up so I'm not embarassed by my dubious code quality.</p> -->
    
    <h3>What's the point?</h3>
    <p>Permissionless data capture and device control over Nostr is cool.</p>

  </div>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  justify-content: space-around;
}
.about {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  max-width: 500px;
}
</style>