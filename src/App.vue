<template>
  <div id="app">
   <div id="nav">
     <router-link to="/">The Dashboard</router-link> |
     <router-link to="/about">What is this thing??</router-link> |
     <a target="_blank" href="https://nostriot.com">nostriot.com</a>
    </div>
    <router-view/>
    <footer>
      <p>Created by <a href="https://iris.to/npub1dqepr0g4t3ahvnjtnxazvws4rkqjpxl854n29wcew8wph0fmw90qlsmmgt">BC</a></p>
      <p><a href="https://github.com/blackcoffeexbt/nostr-weather-dashboard">Project GitHub Repo</a></p>
      <a target="_blank" href="https://nostriot.com">nostriot.com</a>
      </footer>
  </div>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: purple;

    &.router-link-exact-active {
      color: darkpurple;
    }
  }
}

footer {
  display: flex;
  align-content: center;
  justify-content: space-around;
  margin: 50px 0 50px;

  p {
    margin: 0;
  }
}
</style>
