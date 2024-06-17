<template>
  <div id="end-game">
    <audio autoplay>
      <source :src="theSound" type="audio/mp3">
      browser not support audio tag
    </audio>
    <div
      :style="{ '--dominate-color': dominateColor, '--title': `'${title}'` }"
      class="victory"
    ></div>
    <div class="imposter" :style="{ '--light-color': lightColor, '--dark-color': darkColor }">
      <div class="spacesuit">
        <div class="chest-and-head"></div>
        <div class="legs"></div>
        <div class="arm"></div>
        <div class="helmet-glass"></div>
      </div>
    </div>
    <div :style="{ '--bg-color': circleColor }" class="background"></div>
    <div v-show="showBtn">
      <button
        @click="again"
        :style="{ '--dominate-color': dominateColor }"
        class="again-btn a-btn"
      >
        עוד פעם!
      </button>
      <button
        @click="next"
        :style="{ '--dominate-color': dominateColor }"
        class="next-btn a-btn"
      >
        מיציתי
      </button>
    </div>
  </div>
</template>

<script>

export default {
  name: "end-game",
  components: {},
  props: ["isWin", "lightColor", "darkColor"],
  data() {
    return {
      showBtn: false,
    };
  },
  mounted() {
    this.hideBG();
    let myTimer = setTimeout(() => {
      this.showBtn = true;
      clearTimeout(myTimer);
    }, 2500);
  },
  methods: {
    next() {
      this.$emit("toNextPage");
      this.showBG();
    },
    hideBG() {
      this.$emit("hideBackGround");
    },
    again() {
      this.$emit("again");
      this.showBG();
    },
    showBG() {
      this.$emit("showBackGround");
    },
  },
  computed: {
    dominateColor() {
      if (this.isWin) {
        return "#018bfb";
      } else {
        return "#c90108";
      }
    },
    circleColor() {
      if (this.isWin) {
        return "#59a4a3";
      } else {
        return "#b90000";
      }
    },
    title() {
      if (this.isWin) {
        return "Victory";
      } else {
        return "Defeat";
      }
    },
    theSound() {
      
      if (this.isWin) {
        return "../../src/assets/audio/victory-sound.mp3";
      } else {
        return "../../src/assets/audio/defeat-sound.mp3";
      }
    },
  },
};
</script>


<style scoped>
.a-btn {
  background-color: var(--dominate-color);
  padding: 5%;
  position: absolute;
  bottom: 10%;
  border-radius: 5vw;
  color: white;
  border-color: var(--dominate-color);
  font-family: BN Pinky;
  font-size: 10vw;
  width: 40%;
  height: 13%;
  direction: rtl;
  z-index: 999;
 opacity: 0; /* Start with the button hidden */
    animation: fadeIn 2s forwards; /* 2 seconds fade-in animation */
}


@keyframes fadeIn {
    to {
        opacity: 1; /* End state where the button is fully visible */
    }
}

.again-btn {
  right: 5%;
}

.next-btn {
  left: 5%;
}

#end-game {
  margin: 0;
  padding: 0;
  height: 100vh;
  width: 100vw;
  position: relative;
  background-color: #000000;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  z-index: 0;
}

.text {
  font-family: bn pixeliom;
  color: white;
  position: absolute;
  top: 8%;
  left: 50%;
  transform: translateX(-50%);
  font-size: 3vh;
  width: 95%;
  direction: rtl;
}

*,
*:before,
*:after {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}

.victory {
  font-family: "VCR_OSD_MONO", cursive;
  font-size: 6rem;
  font-weight: 800;
  color: var(--dominate-color);
  position: fixed;
  top: 20%;
  z-index: 10;
}

.victory:after {
  content: var(--title);
}

.imposter {
  margin-top: 100px;
  display: flex;
}

.spacesuit {
  position: relative;
}

.chest-and-head {
  width: 140px;
  height: 200px;
  background: var(--dark-color);
  position: relative;
  border: 10px solid #000000;
  border-radius: 60px 80px 0 0;
  border-bottom: none;
}

.chest-and-head:after {
  content: "";
  width: 92%;
  height: 85%;
  background: var(--light-color);
  position: absolute;
  left: 4%;
  /* top: -1px; {*/
  z-index: 2;
  border-radius: 58% 70% 28% 42% / 28% 56% 88% 89%;
}

.legs {
  height: 50px;
  width: 60px;
  position: relative;
  z-index: 2;
  background: var(--dark-color);
  border: 10px solid #000000;
  border-top: none;
  border-radius: 0 0 22px 22px;
}

.legs::after {
  content: "";
  height: 45px;
  width: 60px;
  position: absolute;
  left: 70px;
  background: var(--dark-color);
  border: 10px solid #000000;
  border-top: none;
  border-radius: 0 0 22px 22px;
}

.legs::before {
  content: "";
  height: 10px;
  width: 55px;
  background: var(--dark-color);
  background-color: #000000;
  position: absolute;
  top: -10px;
  left: 40px;
  border-radius: 0 0 10px 0;
}

.arm {
  height: 120px;
  width: 35px;
  background: var(--light-color);
  position: absolute;
  top: 75px;
  left: -35px;
  border: 10px solid #000000;
  border-right: none;
  border-radius: 20px 0 0 22px;
}

.arm:after {
  content: "";
  width: 25px;
  height: 80px;
  background: var(--dark-color);
  position: absolute;
  top: 20px;
  border-radius: 12px 0 0 10px;
}

.helmet-glass {
  width: 110px;
  height: 75px;
  background: #225381;
  position: absolute;
  z-index: 3;
  top: 40px;
  left: 50px;
  border: 10px solid #000000;
  border-radius: 25px 50px 30px 30px;
}

.helmet-glass:after {
  content: "";
  width: 85%;
  height: 65%;
  background: #71d4ec;
  left: 13px;
  position: absolute;
  z-index: 4;
  border-radius: 0 28px 3px 30px;
}

.helmet-glass:before {
  content: "";
  width: 45%;
  height: 22%;
  background: #ffffff;
  position: absolute;
  left: 40px;
  top: 5px;
  z-index: 5;
  border-radius: 10px;
  transform: rotate(6deg);
}

.background {
  width: 80vw;
  height: 20vh;
  margin-top: 30px;
  background: var(--bg-color);
  position: absolute;
  z-index: -1;
  border-radius: 50%;
  box-sizing: border-box;
  box-shadow: 6px 6px 80px 110px var(--bg-color);
}

#end-game:before {
  content: "";
  position: absolute;
  top: 0;
  right: 0;
  width: 200%;
  height: 100%;
  background: linear-gradient(to right, transparent, #000, #000);
  z-index: 30;
  animation: animate 2s linear forwards;
}
@keyframes animate {
  0% {
    right: 0;
  }
  100% {
    right: -200%;
  }
}

@media only screen and (max-width: 900px) {
  .imposter,
  .victory {
    zoom: 60%;
  }
  .background {
    zoom: 0.7;
    margin-top: 10px;
  }
}
</style>