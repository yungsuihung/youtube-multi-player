<template>
  <div :style="style" align="center" justify="center">
    <v-chip
      label
      small
      outlined
      color="#FF000088"
      style="position: absolute; top: 0px; left: 0px"
      >{{ playerId }}</v-chip
    >
    <v-btn
      v-if="submitUrl"
      icon
      @click="submitUrl = ''"
      style="position: absolute; top: 0px; right: 0px"
      x-small
      elevation="2"
      color="#CC876377"
    >
      <v-icon>mdi-close</v-icon>
    </v-btn>
    <v-text-field
      v-if="!submitUrl"
      v-model="rawUrl"
      color="red"
      placeholder="Youtube URL"
      outlined
      clearable
      dense
      class="mt-2"
      style="width: 50%"
    >
      <template slot="append">
        <v-btn
          icon
          color="red"
          @click="submitUrl = rawUrl"
          class="pa-0"
          small
          ><v-icon>mdi-video-plus-outline</v-icon></v-btn
        >
      </template>
    </v-text-field>
    <iframe
      v-if="srcUrl"
      :width="width"
      :height="height"
      :src="srcUrl"
      frameborder="0"
      allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
      allowfullscreen
    ></iframe>
  </div>
</template>

<script>
export default {
  props: ["width", "height", "playerId"],
  data: () => ({
    submitUrl: "",
    rawUrl: "",
  }),
  computed: {
    srcUrl() {
      try {
        var url = new URL(this.submitUrl);
        let code = "";

        if (url) code = url.searchParams.get("v");
        else throw "Invalid URL.";

        if (!code) {
          let path = url.pathname;
          if (path && path != "/watch") {
            code = path.substring(1);
          } else throw "Fail decoding youtube URL.";
        }
        if (code) return "https://www.youtube.com/embed/" + code;
        else throw "Unsupported URL form.";
      } catch (err) {
        console.log(err.message);
      }
      return "";
    },
    style() {
      return `position: relative; height: ${this.height}px;border-style:solid;border-color:#FF000010;border-width:1px;`;
    },
  },
};
</script>

<style>
</style>