<template>
  <!-- eslint-disable  -->
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      Below as you've probably noticed there's a box and lens. Let's drag it and
      see what happens.(focal length is {{ f }} try to adjust it!)
       <input type="range" v-model="f"
         min="-250" max="250"> </p>  
    <svg
      xmlns="http://www.w3.org/2000/svg"
      width="50%"
      height="30%"
      :viewBox="[startX, startY, backgroundWidth, backgroundHeight]"
    >
      <rect
        :width="backgroundWidth"
        :height="backgroundHeight"
        class="background"
      />
      <path
        class="static"
        d="M 20 180 l 65 0 l 0 15 l 15 0 l 0 10 l -15 0 l 0 15 l -65 0 Z"
        style="stroke: white; fill: grey"
      />
      <path
        v-if="!inter.length"
        :key="lensPos.y"
        id="beam1"
        :d="makePath(emiterStart, paths)"
      />
      <path
        v-else
        :key="lensPos.y"
        id="beam2"
        :d="makePath(emiterStart, [[inter[1].x-emiterStart.x, inter[1].y-emiterStart.y], nextPoint(inter[1].x, backgroundWidth , emiterStart.y - (lensPos.y + lensPos.height/2), f)])"
      />
      <!-- {x:inter[1].x, y:inter[1].y} -->
      <rect
        ref="lens1"
        class="draggable"
        :x="lensPos.x"
        :y="lensPos.y"
        :width="lensPos.width"
        :height="lensPos.height"
        style="stroke: none; fill: #33fcff; opacity: 0.5"
        @mousedown="startDrag($event)"
        @mousemove="drag($event)"
        @mouseup="endDrag($event)"
        @mouseleave="endDrag($event)"
      />
    </svg>
    <!-- <button @click="getInfo" @click.ctrl="testModule">test</button>
    <button @click="getIntersection">test2</button> -->

  </div>
</template>

<script>
/* eslint-disable */
// import { svgIntersections } from "./svg-intersections"
// import { svgIntersections } from 'svg-intersections';

var svgIntersections = require("svg-intersections");

export default {
  name: "svgDraw",
  // components: {
  //   svgIntersections
  // },
  props: {
    msg: String,
  },
  data() {
    return {
      startX: 0,
      startY: 0,
      backgroundWidth: 600,
      backgroundHeight: 400,
      paths: [500, 0],
      lensPos: {
        x: 300,
        y: 150,
        width: 5,
        height: 80,
      },
      selected: false,
      offset: null,
      f: 100,
      emiterStart: { x: 100, y: 200 },
      inter: [0, 0],
    };
  },
  computed: {
    h() {
      return this.backgroundHeight / 3;
    },
    path0() {
      return this.backgroundWidth - 100;
    },
    angle(start, end) {},
    emiter() {
      return this.emiterStart;
    },
  },
  methods: {
    getInfo() {
      const test = document.body.getElementsByTagName("rect");
      console.log(test[0]);
    },
    getIntersection(emiterStart, paths, lensPos) {
      var intersect = svgIntersections.intersect;
      var shape = svgIntersections.shape;
      var intersections = intersect(
        shape("path", { d: this.makePath(emiterStart, paths) }),
        shape("rect", lensPos)
      );
      return intersections.points;
    },
    makePath(start, points) {
      // console.log(`M ${start.x} ${start.y} l ${points[0]} ${points[1]}`);
      // console.log("start", start);
      // console.log("points", points);
      if (typeof points[0] != "object"){
        return `M ${start.x} ${start.y} l ${points[0]} ${points[1]}`;}
    
      let rest ="";
      for (let point of points) {
        rest = rest + `l ${point[0]} ${point[1]}`;
        // console.log("good", rest)
        
      }

      return `M ${start.x} ${start.y} ${rest}`;
    },
    getMousePosition(event) {
      const CTM = event.target.getScreenCTM();
      return {
        x: (event.clientX - CTM.e) / CTM.a,
        y: (event.clientY - CTM.f) / CTM.d,
      };
    },

    startDrag(event) {
      if (event.target.classList.contains("draggable")) {
        this.offset = this.getMousePosition(event);
        this.offset.x -= parseFloat(event.target.getAttributeNS(null, "x"));
        this.offset.y -= parseFloat(event.target.getAttributeNS(null, "y"));
        this.selected = true;
      }
    },
    drag(event) {
      if (this.selected == true) {
        event.preventDefault();
        let coords = this.getMousePosition(event);
        let x = parseFloat(event.target.getAttributeNS(null, "x"));
        let y = parseFloat(event.target.getAttributeNS(null, "y"));
        // event.target.setAttributeNS(null, 'x', coords.x - this.offset.x)
        // event.target.setAttributeNS(null, 'y', coords.y - this.offset.y)
        (this.lensPos.x = coords.x - this.offset.x),
          (this.lensPos.y = coords.y - this.offset.y);
        this.inter = this.getIntersection(
          this.emiterStart,
          this.paths,
          this.lensPos
        );
      }
    },
    endDrag(event) {
      this.selected = false;
    },
    nextPoint(current, next, alttitude, f) {
      console.log(alttitude)
      return [next-current,  -(next-current)*alttitude/f]
    }
  },
};
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
.background {
  fill: #42b983;
}
#beam1 {
  stroke: red;
}
#beam2 {
  stroke: red;
  fill:none
}
.static {
  cursor: not-allowed;
}

.draggable {
  cursor: move;
}
</style>
