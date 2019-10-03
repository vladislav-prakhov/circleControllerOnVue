<template>
  <div class="application">
    <h1>x:{{ storeState.canvasCircle.x }}, y:{{ storeState.canvasCircle.y }}</h1>
    <div class="app-box">
      <div class="canvas-table">
        <canvas id="playground" width="500" height="500">
        </canvas>
      </div>
      <div class="controller-pad">
        <div class="control-container">
          <div class="control-top-part">
            <button id="top-button" class="control-button" v-on:click="animate('top')">
              <font-awesome-icon icon="arrow-up" />
            </button>
          </div>
          <div class="control-bottom-part">
            <button id="left-button" class="control-button" v-on:click="animate('left')">
              <font-awesome-icon icon="arrow-left" />
            </button>
            <button id="bottom-button" class="control-button" v-on:click="animate('bottom')">
              <font-awesome-icon icon="arrow-down" />
            </button>
            <button id="right-button" class="control-button" v-on:click="animate('right')">
              <font-awesome-icon icon="arrow-right" />
            </button>
          </div>
        </div>
      </div>

    </div>
    <footer></footer>
  </div>
</template>

<script>
import { store } from './Store.vue';

export default {
  name: 'RoundController',
  props: {
    name: String,
  },

  data() {
    return {
      storeState: store.state
    };
  },

  methods: {
    initialDraw: function () {
      let canvas = document.getElementById('playground');

      if (canvas.getContext) {
        let ctx = canvas.getContext('2d');
        // drawing code here

        ctx.beginPath();
        ctx.arc(store.state.canvasCircle.x, store.state.canvasCircle.y, 60, 0, Math.PI * 2, true); // Outer circle
        ctx.stroke();
      }
    },
    animate: function animate(direction) {
      let x =store.state.canvasCircle.x;
      let y = store.state.canvasCircle.y;
      let step =50;
      let stepCount = step;

      let canvas = document.getElementById('playground');

      if (canvas.getContext) {
        let ctx = canvas.getContext('2d');

        let circleAnimation = setInterval( function () {
          if (direction === 'left') {
            if (x === 60) document.getElementById("top-button").disabled = true;
            x--;
          }
          if (direction === 'right') {
            x++;
          }
          if (direction === 'top') {
            y--;
          }
          if (direction === 'bottom') {
            y++;
          }
          stepCount--;
          // Clear
          ctx.clearRect(0, 0, canvas.width, canvas.height);

          // Draw
          ctx.beginPath();
          ctx.arc(x, y, 60, 0, Math.PI * 2, true); // Outer circle
          ctx.stroke();

          if (direction === 'left' && (stepCount===0 || x === 60)) {
            clearInterval(circleAnimation);
            let newX = store.state.canvasCircle.x - step + stepCount;
            let newY = store.state.canvasCircle.y;
            store.changePosition(newX, newY);
          }
          if (direction === 'right' && (stepCount===0 || x===440)) {
            clearInterval(circleAnimation);
            let newX = store.state.canvasCircle.x + step - stepCount;
            let newY = store.state.canvasCircle.y;
            store.changePosition(newX, newY);
          }
          if (direction === 'top' && (stepCount===0 || y === 60)) {
            clearInterval(circleAnimation);
            let newX = store.state.canvasCircle.x;
            let newY = store.state.canvasCircle.y - step + stepCount;
            store.changePosition(newX, newY);
          }
          if (direction === 'bottom' && (stepCount===0 || y===440)) {
            clearInterval(circleAnimation);
            let newX = store.state.canvasCircle.x;
            let newY = store.state.canvasCircle.y+step-stepCount;
            store.changePosition(newX, newY);
          }
          document.getElementById("left-button").disabled = (x === 60);
          document.getElementById("right-button").disabled = (x === 440);
          document.getElementById("top-button").disabled = (y === 60);
          document.getElementById("bottom-button").disabled = (y === 440);
        }, 5);


      }
    },
    clrScr : function () {
      let canvas = document.getElementById('playground');

      if (canvas.getContext) {
        let ctx = canvas.getContext('2d');
        // drawing code here
        ctx.clearRect(0,0,canvas.width, canvas.height);
        alert(this.$props.circle.x);
      }
    }
  },
  mounted(){
    this.initialDraw()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.app-box {
  margin: 0 auto;
  width: 1000px;
  background: #efefef;
  height: 500px;
  display: flex;
}
.canvas-table {
  flex:1;

}
.controller-pad {
  flex:1;
  display: flex;
  background: lightblue;
}
.control-container {
  width: 240px;
  height: 160px;
  margin: auto auto;
  background: peachpuff;
  display: flex;
  flex-direction: column;
}
.control-top-part {
  flex: 1;
  display: flex;
  justify-content: space-around;
}

.control-bottom-part {
  flex: 1;
  display: flex;
  justify-content: space-around;
}

.control-button {
  margin: auto;
  padding: 10px;
  width: 60px;
  height: 60px;
  background: #eee;
}
footer {
  height: 100px;
  width: 100%;
}

</style>
