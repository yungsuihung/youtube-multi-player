<template>
  <v-app>
    <v-btn
      icon
      color="#7C876377"
      elevation="2"
      class="ma-2"
      @click="settingDialog = !settingDialog"
      style="position: fixed; z-index: 999"
    >
      <v-icon>mdi-cog-outline</v-icon>
    </v-btn>
    <v-dialog v-model="settingDialog" max-width="500" class="pa-5">
      <v-card>
        <v-card-title class="headline grey lighten-2"> Settings </v-card-title>
        <v-card-text class="pt-3">
          <v-select v-model="rowNum" label="Rows" :items="numList"></v-select>
          <v-select
            v-model="colNum"
            label="Columns"
            :items="numList"
          ></v-select>
          <v-slider
            label="Width Adjust"
            v-model="widthAdjust"
            max="50"
            min="-50"
          ></v-slider>
          <v-slider
            label="Height Adjust"
            v-model="heightAdjust"
            max="200"
            min="-200"
          ></v-slider>
        </v-card-text>
      </v-card>
    </v-dialog>
    <v-row v-for="r in rowNum" :key="r" no-gutters>
      <v-col v-for="c in colNum" :key="c">
        <EmbededPlayer
          :width="width"
          :height="height"
          :playerId="(r - 1) * colNum + c"
      /></v-col>
    </v-row>
  </v-app>
</template>

<script>
import EmbededPlayer from "./components/EmbededPlayer.vue";

export default {
  name: "App",
  components: {
    EmbededPlayer,
  },
  data: () => ({
    rowNum: 1,
    colNum: 1,
    numList: [1, 2, 3, 4, 5],
    settingDialog: false,
    widthAdjust: 0,
    heightAdjust: 0,
  }),
  methods: {
    closeCode: (evt, code) => {
      console.log(code);
    },
  },
  computed: {
    width() {
      var width = window.screen.availWidth / this.colNum + this.widthAdjust;
      return width;
    },
    height() {
      var height = window.screen.availHeight / this.rowNum + this.heightAdjust;
      return height;
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
