<template>
  <div id="app">
    <div>RASPBERRY POCKETS</div>

    <div class="grid-container">
      <div id="1" class="grid-item">1</div>
      <div id="2" class="grid-item">2</div>
      <div id="3" class="grid-item">3</div>  
      <div id="4" class="grid-item">4</div>
      <div id="5" class="grid-item">5</div>
      <div id="6" class="grid-item dropzone6">6</div>  
      <div id="7" class="grid-item">7</div>
      <div id="8" class="grid-item">8</div>
      <div id="9" class="grid-item">9</div>  
    </div>

    <div>
      <img id="drip_1" class="drip draggable" dataSnap="2" :src="require('./assets/cool-drip.png')"/> 
    </div>
    <div>
      <img id="drip_2" class="drip drip2 draggable " dataSnap="6" :src="drip2"/>
    </div>
      
    

  </div>
</template>

<script>

import interact from 'interact.js';

export default {
  name: 'App',
  components: {},
  data(){
    return {
      drip2: require('./assets/pink-drip.png'),
      pinkDrip: require('./assets/pink-drip.png'),
      shrimp: require('./assets/shrimp.png')
      
    }
  },
  mounted() {
    const app = document.getElementById('app');
    interact('.draggable').draggable({
      onmove: this.dragMoveListener,
      onstart: this.findSnapLocation,
      inertia: false,
      restrict: {
        restriction: app,
        elementRect: { top: 0, left: 0, bottom: 1, right: 1},
        endOnly: false
      },
    });
    interact('.dropzone6').dropzone({
      accept: '.drip2',
      overlap: .5,
      ondrop: this.switchElement
    });
  },
  computed: {},
  methods: {
    switchElement() {
      this.drip2 = this.shrimp
    },
    findSnapLocation(event) {
      const snapLocation = document.getElementById(event.target.getAttribute('dataSnap'));
      let snapLocationRectangle = snapLocation.getBoundingClientRect();
      let snapPostion = {
        x: snapLocationRectangle.left + snapLocationRectangle.width  / 2,
        y: snapLocationRectangle.top  + snapLocationRectangle.height / 2,
      }

      let homeLocationRectangle = event.target.parentElement.getBoundingClientRect();
      let homePosition = {
        x: homeLocationRectangle.left + homeLocationRectangle.width  / 2,
        y: homeLocationRectangle.top  + homeLocationRectangle.height / 2
      }

      event.interactable.draggable({
        snap: {
          targets: [snapPostion, homePosition],
          relativePoints: [ { x: 0.5, y: 0.5 } ],
          endOnly: true,
          range: 100,
        }
      })
    },
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

      this.drip2 = this.pinkDrip;
    }
  }
}
</script>







