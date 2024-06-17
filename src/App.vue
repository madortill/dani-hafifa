<template>
  <div id="app">
    <space-background v-if="showBG"></space-background>
    <open-page v-if="pageNum === 0" @next="nextPage"></open-page>
    <intro
    :lightColor="lightColor"
    :darkColor="darkColor"
      @setTheColor="setChosenColor"
      v-else-if="pageNum === 1"
      @next="nextPage"
    ></intro>
    <the-ship
    :lightColor="lightColor"
    :darkColor="darkColor"
      @toGame="nextPage"
      :chosenColor="color"
      v-else-if="pageNum === 2"
    ></the-ship>
    <game
    :lightColor="lightColor"
    :darkColor="darkColor"
      @toUnshowBG="noBG"
      @toshowBG="showTheBG"
      :theColor="color"
      @toFinishPage="nextPage"
      v-else-if="pageNum === 3"
    ></game>
    <finish-page
      @toFinishPage="nextPage"
      v-else-if="pageNum === 4"
    ></finish-page>
    <div id="hide"></div>
  </div>
</template>

<script>
import Intro from "./components/Intro.vue";
import OpenPage from "./components/OpenPage.vue";
import SpaceBackground from "./components/SpaceBackground.vue";
import TheShip from "@/components/TheShip.vue";
import Game from "./components/Game.vue";
import FinishPage from "./components/FinishPage.vue";

export default {
  name: "app",
  components: {
    SpaceBackground,
    OpenPage,
    Intro,
    TheShip,
    Game,
    FinishPage,
  },
  data() {
    return {
      pageNum: 0,
      color: "red",
      showBG: true,
      darkColor: "#852631",
      lightColor: "#eb432f",
    };
  },
  methods: {
    nextPage() {
      this.pageNum++;
    },

    setChosenColor(chosen) {
      this.color = chosen;
      this.setUsedColors(chosen);
    },


    noBG() {
      this.showBG = false;
    },

    showTheBG() {
      this.showBG = true;
    },

      setUsedColors(theColor) {
      switch (theColor) {
        case "red":
          this.lightColor = "#eb432f";
          this.darkColor = "#852631";
          break;
        case "lightblue":
          this.lightColor = "#38ffdd";
          this.darkColor = "#35a7c0";
          break;
        case "blue":
          this.lightColor = "#1125da";
          this.darkColor = "#070b93";
          break;
        case "green":
          this.lightColor = "#61ef00";
          this.darkColor = "#2da82a";
          break;
        case "darkgreen":
          this.lightColor = "#158231";
          this.darkColor = "#115335";
          break;
        case "yellow":
          this.lightColor = "#f3f658";
          this.darkColor = "#c28722";
          break;
        case "brown":
          this.lightColor = "#71491e";
          this.darkColor = "#5e2615";
          break;
        case "orange":
          this.lightColor = "#f07d0d";
          this.darkColor = "#b43e15";
          break;
      }
    },
  },
};
</script>

<style>
@font-face {
  font-family: "BN Pinky";
  src: url("@/assets/fonts/BN Pinky.ttf");
}

@font-face {
  font-family: "bn pixeliom";
  src: url("@/assets/fonts/bn pixeliom.ttf");
}

@font-face {
  font-family: "VCR_OSD_MONO";
  src: url("@/assets/fonts/VCR_OSD_MONO.ttf");
}

html,
body {
  margin: 0;
  overflow: hidden;
  font-family: BN Pinky;
  color: white;
}

@media screen and (width> 770px) {
  #hide {
    display: block;
    z-index: 999;
    width: 100vw;
    height: 100vh;
    background-color: black;
    position: absolute;
    top: 0%;
  }
}
</style>
