<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>Below as you've probably noticed there are three boxes.</p>
    <svg
      xmlns="http://www.w3.org/2000/svg"
      width="50%"
      height="30%"
      :viewBox="[startX, startY, backgroundWidth, backgroundHeight]"
    >
      <rect :width="backgroundWidth" :height="backgroundHeight" class="background" />
    </svg>
    <button @click="getInfo">test</button>
  </div>
</template>

<script>
export default {
  name: 'svgDraw',
  props: {
    msg: String,
  },
  data() {
    return {
      startX: 0,
      startY: 0,
      backgroundWidth: 500,
      backgroundHeight: 300,
      h: this.backgroundHeight / 3,
    };
  },
  methods: {
    getInfo() {
      const test = document.body.getElementsByTagName('rect');
      console.log(test[0]);
    },
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
</style>
