<template>
  <div id="root">
    <router-view/>
  </div>
</template>

<style lang="scss">
*, *::before, *::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#root {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;

  color: #2c3e50;
  background-color: rgb(20, 20, 20);

  min-height: 100vh;
  min-width: 100vw;
  max-height: 100vh;
  max-width: 100vw;

  display: flex;
  justify-content: center;
  align-items: center;
  align-content: stretch;
  overflow: hidden;
  position: relative;
}
</style>
