<template>
  <div id="app">
    <div class="nav">
      <ul>
        <router-link tag="li" to="/" active-class="active" exact><a>PHOTOBOOTH</a></router-link>
        <router-link tag="li" to="/gallery" active-class="active"><a>Gallery</a></router-link>
        <router-link tag="li" to="/about" active-class="active"><a>About</a></router-link>
      </ul>
    </div>
    <router-view/>
  </div>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

body{
  margin: 0px;
}

.nav {
  background-color: rgb(216, 16, 76);
  margin-top: -15px;
}

ul {
  display: flex;
}

.routes-container {
}

ul li {
  list-style-type: none;
  padding: 10px;
}

ul li a {
  text-decoration: none;
  color: #fff;
}

.active a {
  border-bottom: 2px solid #000;
}

</style>
