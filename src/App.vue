<template>
  <v-app id="my-app">
    <sg-component1
      :exposed="exposed1"
      @exposed="exposedChanged1"
    ></sg-component1>
    <sg-component2 @exposed="exposedChanged2"></sg-component2>
    <sg-component3
      :country-name="country"
      @country-name="countryChanged"
      @center-coords="centerCoordsChanged"
    ></sg-component3>
    <!-- remove debug v-card before build -->
    <v-card class="ma-2">
      <v-card-title primary-title>
        <div>
          <h4 class="title mb-0">App Debug Area</h4>
          <h4 class="ma-1">Remove before build</h4>
          <div>
            exposed1:
            {{ exposed1 }}
            <br /><br />
            exposed2:
            {{ exposed2 }}
          </div>
        </div>
      </v-card-title>
    </v-card>
  </v-app>
</template>

<script>
import Component1 from './components/component1/Component1';
import Component2 from './components/component2/Component2';
import Component3 from './components/component3/Component3';
import _ from 'lodash';

export default {
  name: 'App',
  components: {
    'sg-component1': Component1,
    'sg-component2': Component2,
    'sg-component3': Component3
  },
  data: function() {
    return {
      exposed1: { min: 180, max: 333 },
      exposed2: null,
      country: null,
      centerCoords: null
    };
  },
  methods: {
    exposedChanged1: function(v) {
      /* prevent update loop */
      if (!_.isEqual(this.exposed1, v)) {
        this.exposed1 = v;
      }
    },
    exposedChanged2: function(v) {
      this.exposed2 = v;
    },
    countryChanged(v) {
      /* Prevent update loop */
      if (v === this.country) {
        return;
      }
      this.country = v;
    },
    centerCoordsChanged(v) {
      this.centerCoords = v;
    }
  }
};
</script>
