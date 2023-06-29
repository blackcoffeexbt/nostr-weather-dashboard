<template>
  <div id="app">
   <div id="nav">
     <router-link to="/">Dashboard</router-link> |
     <router-link to="/about">What is this thing??</router-link>
    </div>
    <router-view/>
    <footer>
      <p>Created by <a href="https://iris.to/npub1dqepr0g4t3ahvnjtnxazvws4rkqjpxl854n29wcew8wph0fmw90qlsmmgt">BC</a></p>
      <p><a href="https://github.com/blackcoffeexbt/nostr-weather-dashboard">Project GitHub Repo</a></p>
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
</style>
