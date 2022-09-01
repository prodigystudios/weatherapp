<template>
  <router-view />
</template>

<style>
html
{
  overflow: hidden;
  scroll-behavior: smooth;
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
    scroll-behavior: smooth;
  }
  #app {
    font-family: fantasy;
    letter-spacing: 5px;
  }
}

@media (max-width: 650px) {
  html {
    overflow:visible;
    scroll-behavior: smooth;
  }
  #app {
    font-family: fantasy;
    letter-spacing: 5px;
  }
}
@media screen and (orientation: landscape) {
  html {
    overflow: visible;
  }
}
</style>
