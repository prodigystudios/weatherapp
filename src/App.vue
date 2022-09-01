<template>
  <router-view />
</template>

<style>
html
{
  overflow: hidden;
}

body
{
  margin: 0;
}

#app
{
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  font-family: cursive;
 
}

@media (max-width: 425px) {
  html {
    overflow:visible;
  }
  #app {
    font-family: fantasy;
    letter-spacing: 5px;
  }
}
</style>
