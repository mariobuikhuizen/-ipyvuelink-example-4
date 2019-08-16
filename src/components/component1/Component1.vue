<template>
  <div class="main">
    <v-container fluid class="debug-border">
      <v-layout flex row ma-1 :style="style.total" class="debug-border">
        <div :style="style.input">
          <v-text-field
            v-model="extent[0]"
            class="mr-2 my-dense"
            hide-details
            single-line
            type="number"
            :step="config.step"
          ></v-text-field>
        </div>

        <v-range-slider
          class="my-dense"
          v-model="extent"
          :max="config.max"
          :min="config.min"
          :step="config.step"
        ></v-range-slider>

        <div :style="style.input">
          <v-text-field
            v-model="extent[1]"
            class="ml-2 my-dense"
            hide-details
            single-line
            type="number"
            :step="config.step"
          ></v-text-field>
        </div>
      </v-layout>
    </v-container>

    <!-- remove debug v-card before build -->
    <v-card class="ma-2">
      <v-card-title primary-title>
        <div>
          <h4 class="title mb-0">Example1 Debug Area</h4>
          <h4 class="ma-1">Remove before build</h4>
          <div>
            config:
            {{ config }}
            <br />
            width:
            {{ width }}
            <br />
            extent:
            {{ extent }}
          </div>
        </div>
      </v-card-title>
    </v-card>
  </div>
</template>

<script>
export default {
  props: {
    // exposed
    exposed: Object
  },
  data() {
    return {
      config: { max: 500, min: 100, step: 5 },
      width: { total: 400, input: 70 },
      //local
      extent: []
    };
  },
  computed: {
    style() {
      return {
        total: `width: ${this.width.total}px`,
        input: `width: ${this.width.input}px`
      };
    }
  },
  watch: {
    extent: {
      handler: function(v) {
        this.$emit('exposed', {
          min: v[0],
          max: v[1]
        });
      }
    },
    exposed: {
      handler: function(v) {
        this.extent = [v.min, v.max];
      }
    }
  },
  created() {
    const shift = (this.config.max - this.config.min) / 4;

    this.extent = [
      this.exposed.min || this.config.min + shift,
      this.exposed.max || this.config.max - shift
    ];
  }
};
</script>

<style scoped>
.main {
  background-color: white;
}
/* remove before build */
.debug-border {
  border: 1px solid red;
}

.my-dense {
  margin: 0px;
  padding: 0px;
}
.v-input--range-slider >>> .v-messages {
  /* background-color: red; */
  display: none;
}
.v-text-field >>> {
  font-size: 18px;
}
</style>
