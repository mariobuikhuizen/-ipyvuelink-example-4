<template>
  <v-container fluid id="app" class="ma-0 debug-border" :style="style.app">
    <Globe />
    <Controls />
  </v-container>
</template>

<script>
import store from './store';
import Globe from './Globe.vue';
import Controls from './Controls.vue';

export default {
  components: {
    Globe,
    Controls
  },
  beforeCreate() {
    this.$store = store();
  },
  props: { countryName: String },
  data() {
    return {
      //   config
      size: { height: 550, ratio: 1.6 }
      // technical
      // dataModel: null,
      // dataAvailable: false
    };
  },
  computed: {
    style() {
      return {
        globe: {
          width: this.size.height,
          height: this.size.height
        },
        controls: {
          width: Math.round(this.size.height * this.size.ratio - 1),
          height: this.size.height
        },
        app: {
          width: Math.round(this.size.height * this.size.ratio) + 'px',
          height: this.size.height + 'px'
        }
      };
    },
    exposed() {
      return this.$store.getters['exposed'];
    },
    countryLocal() {
      return this.$store.getters['selected'];
    },
    centerCoords() {
      return this.$store.getters['centerCoords'];
    }
  },
  created() {
    this.$store.dispatch('setStyle', this.style);
  },
  watch: {
    exposed: {
      handler: function() {
        console.log(this.exposed);
        // this.dataModel.set('exposed', this.exposed);
        // this.dataModel.save_changes();
        // this.dataModel.touch();
      },
      immediate: true,
      deep: true
    },
    countryLocal: {
      handler: function(v) {
        this.$emit('country-name', v);
      }
    },
    countryName: {
      handler: function(v) {
        this.$store.dispatch('setSelected', v);
      }
    },
    centerCoords: {
      handler: function(v) {
        this.$emit('center-coords', v);
      }
    }
  }
};
</script>

<style scoped></style>
