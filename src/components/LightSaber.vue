<template>
  <div class="light-saber" style="display: flex; position: relative; height: 80px; overflow: visible">
    <img class="handle" alt="lightsaber handle" src="@/assets/lightsaber-handle.svg" style="transform: translateY(-40px)">
    <canvas class="blade-halo" style="position: absolute;z-index: -2"/>
    <p class="team-name" style="text-align: start">{{ team }}</p>
    <div class="blade" style="left:0; height: 16px; position: absolute; z-index: -1; background-color: white; border-radius: 16px; transform: translateY(-50%)"></div>
    <p :style="{color: color, paddingLeft: blade_total_length + 20 + 'px'}" style="margin: 0; font-family: Starjedi; align-items:center; line-height: 0px; font-size: 50px; transform: translateY(-5%)">{{this.score > 0 ? score : 0}}</p>
  </div>
</template>

<script>
import DrawBlob from 'blob-animated';

export default {
  name: "LightSaber",
  props: {
    score: Number,
    color: String,
    team: String
  },
  watch: {
    score: function() {
      this.generateSaber();
    }
  },
  data() {
    return {
      blade_total_length: 1
    }
  },
  mounted () {
    this.generateSaber();
  },
  methods: {
    generateSaber: function (){
      //Not vue architecture, must redo
      const component = this.$el
      const canvas = component.getElementsByClassName('blade-halo')[0];
      const blade = component.getElementsByClassName('blade')[0];
      const handle = component.getElementsByClassName('handle')[0];
      let vect = []
        let blade_halo_total_length =  this.score > 0 ? this.score + 300 : 0;
      let handle_width = 150;
      let distance_between_waves = 100;
      let quant = Math.round(blade_halo_total_length / distance_between_waves);
      let decal = (blade_halo_total_length / quant);
      let blade_total_length = (blade_halo_total_length - decal * 1.8);
      let blade_halo_thickness = 80;
      let blade_thickness_percentage =  blade_halo_thickness / blade_halo_total_length
      this.blade_total_length = blade_total_length
      canvas.style.left = (handle_width - decal) - 10 + "px"
      canvas.style.width = blade_halo_total_length + "px"
      canvas.style.height = blade_halo_total_length + "px"
      canvas.style.top = - (blade_halo_total_length  / 2) + "px"
      blade.style.width = blade_total_length + "px";
      blade.style.left = handle_width - handle_width / 8 + "px";
      handle.style.width = handle_width + "px";


      function rdmInterval(min, max) {
        return Math.random() * (max - min) + min;
      }
      for (let i = 1; i <= quant - 1; ++i) {
        let thickness_ratio = (quant - i) / quant;
        let rand = i === 2 ? 0 : rdmInterval(-0.01 *(1-thickness_ratio),0.01 * (1-thickness_ratio));
        vect.push({ y:  0.5 + blade_thickness_percentage / 2 - thickness_ratio * (blade_thickness_percentage / 4) + rand, x: thickness_ratio})
      }
      vect.push({y: 0.5, x: 0.7 / quant})
      for (let i = quant - 1; i >= 1;--i) {
        let thickness_ratio = (quant - i) / quant;
        let rand = i === 2 ? 0 : rdmInterval(-0.01 *(1-thickness_ratio),0.01 * (1-thickness_ratio));
        vect.push({ y: 0.5 - blade_thickness_percentage / 2 + thickness_ratio * (blade_thickness_percentage / 4) + rand, x: thickness_ratio})
      }
      vect.push({y: 0.5, x: 1 - 0.75 / quant})


      new DrawBlob({
        canvas: canvas,
        speed: 100,
        scramble: 0.007 * 500 / blade_halo_total_length,
        color: this.color,
        vectors: vect
      });
    }
  }
}

</script>

<style scoped>
@font-face {
  font-family: "Starjedi";
  src: local("Starjedi"),
  url(@/assets/fonts/Starjedi.ttf) format("truetype");
}

.team-name {
  font-family: "Starjedi", Helvetica, Arial, serif;
  font-size: 100px;
  z-index: -1;
  position: absolute;
  padding: 0;
  margin: 0;
  color: #E9E7E9;
  transform: translateY(-80%) translateX(-50px);
}

.light-saber {
  margin: 100px 100px 0 100px;
}

</style>