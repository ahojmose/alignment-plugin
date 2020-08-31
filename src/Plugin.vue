<template>
  <div class="container">
    <div class="button-group">
      <button
        v-for="breakpoint in breakpoints"
        :class="{ active: currBreakpoint === breakpoint.size }"
        v-on:click="setBreakpoint(breakpoint.size)"
        v-bind:key="breakpoint.size"
      >{{breakpoint.name}}</button>
    </div>

    <div
      v-for="breakpoint in breakpoints"
      v-if="currBreakpoint === breakpoint.size"
      :class="['box-model', breakpoint.size]"
      v-bind:key="breakpoint.size"
    >
      <div class="box-model__margin relative">
        
        <div class="tree__form-group">
          
          <span class="form__topic">Content Horizontal Alignment</span>
          
          <select
            class="uk-width-1-1"
            v-if="model[breakpoint.size]"
            v-model="model[breakpoint.size].x"
          >
            <option :key="option" v-for="option in optionsHorizontal" :value="option">
              {{
              option
              }}
            </option>
          </select>
        </div>
        
        <div class="tree__form-group">
          
          <span class="form__topic">Content Vertical Alignment</span>
          <select
            class="uk-width-1-1"
            v-if="model[breakpoint.size]"
            v-model="model[breakpoint.size].y"
          >
            <option :key="option" v-for="option in options" :value="option">
              {{
              option
              }}
            </option>
          </select>
        </div>

      </div>
    </div>
  </div>
</template>

<script>
import { breakpoints } from "./datasource";

export default {
  mixins: [window.Storyblok.plugin],
  data: function() {
    return {
      breakpoints,
      options: ["-", "start", "center", "end"],
      optionsHorizontal: ["-", "start", "center", "end", "stretch"],
      currBreakpoint: "s"
    };
  },
  methods: {
    initWith() {
      return {
        // needs to be equal to your storyblok plugin name
        plugin: "alignment-plugin",
        ...breakpoints.reduce((acc, curr) => {
          return {
            ...acc,
            [curr.size]: {
              x: "-",
              y: "-",
            }
          };
        }, {})
      };
    },
    setBreakpoint(bp) {
      this.currBreakpoint = bp;
    },
    pluginCreated() {
      // eslint-disable-next-line
      console.log(
        "View source and customize: https://github.com/storyblok/storyblok-fieldtype"
      );
    }
  },
  watch: {
    model: {
      handler: function(value) {
        this.$emit("changed-model", value);
      },
      deep: true
    }
  }
};
</script>

<style>
.active {
  background-color: #efefef;
}

.container {
  display: flex;
  flex-direction: column;
}

.button-group {
  margin: 8px 0 32px 0;
  display: flex;
  align-self: flex-end;
}
</style>
