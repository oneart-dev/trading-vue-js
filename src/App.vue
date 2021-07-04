<template>
  <div>
<trading-vue :data="chart" :width="this.width" :height="this.height"
        :color-back="colors.colorBack"
             :toolbar="true"
             @tool-drag="event"
             @hide-settings="hideSettings"
             @show-settings="objectSelected"
        :color-grid="colors.colorGrid"
        :color-text="colors.colorText">
</trading-vue>
    <div v-if="settings" style="position: absolute; top: 100px; left: 100px; z-index: 1000">
      set color
      <button type="button" @click="setColor">
        red
      </button>
    </div>
  </div>
</template>

<script>
import TradingVue from './TradingVue.vue'
import Data from '../data/data.json'
import DataCube from '../src/helpers/datacube.js'

export default {
    name: 'app',
    components: {
        TradingVue
    },
    methods: {
      setColor() {
        console.log(this.chart.merge(this.settings+'.settings', {
          color: 'red'
        }));
      },
      onResize() {
            this.width = window.innerWidth
            this.height = window.innerHeight
        },
      event(...event) {
          console.log('event', event);
      },
      objectSelected(uuid) {
        this.settings = uuid;
          console.log('show settings', uuid);
      },
      hideSettings(...event) {
        this.settings = false;
        console.log('event hideSettings', event);
      }
    },
    mounted() {
        window.addEventListener('resize', this.onResize)
        window.dc = this.chart
    },
    beforeDestroy() {
        window.removeEventListener('resize', this.onResize)
    },
    data() {
        return {
            settings: false,
            chart: new DataCube(Data),
            width: window.innerWidth,
            height: window.innerHeight,
            colors: {
                colorBack: '#fff',
                colorGrid: '#eee',
                colorText: '#333',
            }
        };
    }
};
</script>

<style>
html,
body {
    background-color: #000;
    margin: 0;
    padding: 0;
    overflow: hidden;
}
</style>
