<template>
  <div id="app">
    <img class="draggable" :src="require('./assets/cool-drip.png')"/> 
    <img class="draggable" :src="require('./assets/pink-drip.png')"/> 
    <div>RASPBERRY POCKETS</div>
  </div>
</template>

<script>
import interact from 'interact.js';

export default {
  name: 'App',
  components: {},
  data(){
    return {}
  },
  mounted() {
    interact('.draggable').draggable({
      onmove: this.dragMoveListener,
      inertia: false,
    });
  },
  methods: {
    dragMoveListener(event) {
      var target = event.target,
          x = (parseFloat(target.getAttribute('data-x')) || 0) + event.dx,
          y = (parseFloat(target.getAttribute('data-y')) || 0) + event.dy;
      // translate the element's CSS/Styling (so that it can move starting from the line above data positioning)
      target.style.webkitTransform = 'translate(' + x + 'px, ' + y + 'px)';
      target.style.transform = 'translate(' + x + 'px, ' + y + 'px) rotate('+ '0' + 'deg)';
      // update the posiion attributes so that it keeps moving
      target.setAttribute('data-x', x);
      target.setAttribute('data-y', y);
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  height: 100vh;
  touch-action: none;
  overflow: hidden;
}
img {
  width: 300px;
}
</style>
