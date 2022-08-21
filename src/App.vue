<template>
<router-view/>
<video autoplay loop muted>
  <source src="./assets/AnimatedBackground/Test.mp4" width="1920px">
</video>
</template>

<style>

html {
  overflow: hidden;
}
body {
  margin:0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  font-family:cursive;
}
source {
  object-fit: fill;
}
nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}

@media (max-width: 425px) {
  source {
  object-fit: fill;
}  
}
</style>
