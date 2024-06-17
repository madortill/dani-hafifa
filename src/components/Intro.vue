<template>
  <div id="intro">
    <div v-if="showPart === 1" class="pick-color">
      <p class="text">תבחרי את צבע השחקן שלך:</p>
      <div
        :style="{ '--light-color': lightColor, '--dark-color': darkColor }"
        class="imposter"
      >
        <div class="spacesuit">
          <div class="chest-and-head"></div>
          <div class="legs"></div>
          <div class="arm"></div>
          <div class="helmet-glass"></div>
        </div>
      </div>

      <div>
        <table class="color-container">
          <tr>
            <td>
              <img
                class="color" 
                :style="{ boxShadow: glowEffects['red'] ? '0 0 20px yellow' : 'none' }"
                @click="setColor('red')"
                src="src/assets/pictures/colorBTN/redBTN.svg"
                alt="color"
              />
            </td>
            <td>
              <img
                class="color"
                :style="{ boxShadow: glowEffects['brown'] ? '0 0 20px yellow' : 'none' }"
                @click="setColor('brown')"
                src="src/assets/pictures/colorBTN/brownBTN.svg"
                alt="color"
              />
            </td>
            <td>
              <img
                class="color"
                :style="{ boxShadow: glowEffects['green'] ? '0 0 20px yellow' : 'none' }"
                @click="setColor('green')"
                src="src/assets/pictures/colorBTN/greenBTN.svg"
                alt="color"
              />
            </td>
            <td>
              <img
                class="color"
                :style="{ boxShadow: glowEffects['lightblue'] ? '0 0 20px yellow' : 'none' }"
                @click="setColor('lightblue')"
                src="@/assets/pictures/colorBTN/lightblueBTN.svg"
                alt="color"
              />
            </td>
          </tr>
          <tr>
            <td>
              <img
                class="color"
                 :style="{ boxShadow: glowEffects['orange'] ? '0 0 20px yellow' : 'none' }"
                @click="setColor('orange')"
                src="@/assets/pictures/colorBTN/orangeBTN.svg"
                alt="color"
              />
            </td>
            <td>
              <img
                class="color"
                :style="{ boxShadow: glowEffects['yellow'] ? '0 0 20px yellow' : 'none' }"
                @click="setColor('yellow')"
                src="@/assets/pictures/colorBTN/yellowBTN.svg"
                alt="color"
              />
            </td>
            <td>
              <img
                class="color"
                :style="{ boxShadow: glowEffects['darkgreen'] ? '0 0 20px yellow' : 'none' }"
                @click="setColor('darkgreen')"
                src="@/assets/pictures/colorBTN/darkgreenBTN.svg"
                alt="color"
              />
            </td>
            <td>
              <img
                class="color"
                 :style="{ boxShadow: glowEffects['blue'] ? '0 0 20px yellow' : 'none' }"
                @click="setColor('blue')"
                src="@/assets/pictures/colorBTN/blueBTN.svg"
                alt="color"
              />
            </td>
          </tr>
        </table>
      </div>
    </div>
    <div v-else class="instructions">
      <!-- <audio ref="audioElement" loop>
        <source src="@/assets/audio/among-us-typing.mp3" type="audio/mpeg">
        your browser does not support audio tag
      </audio> -->
      <div class="finale-exe type-writer">
        <p
          v-for="(text, index) in array1"
          :key="text"
          :style="{
            '--delay': `${index * 2}s`,
            '--width': `${text.length}ch`,
          }"
          class="text-writer"
        >
          {{ text }}
        </p>
      </div>
    </div>
    <img
      v-if="showArrow"
      class="arrow"
      @click="next"
      src="@/assets/pictures/arrow.svg"
      alt="arrow"
    />
  </div>
</template>

<script>
export default {
  name: "intro",
  props: ["lightColor", "darkColor"],
  data() {
    return {
 selectedColor: '', // Track the currently selected color
      glowEffects: {     // Object to track which colors have glowing effect
        red: false,
        brown: false,
        green: false,
        lightblue: false,
        orange: false,
        yellow: false,
        darkgreen: false,
        blue: false,
      },
      showPart: 1,
      color: "red",
      showArrow: true,
      array1: [
        "עשרות אלפי אסטרואידים ",
        "נעים ברגע זה לכיוון ",
        "הכוכב שלנו, ועליך ",
        "לדעת כיצד למנוע מהם ",
        "להגיע אליו.",
        " בהמשך תראי חדרים בהם  ",
        " יהיה פריט בוהק אליו ",
        "תצטרכי להגיע כל פעם,",
        "בהצלחה סומכים עליך!!",
      ],
      
    };
  },
  methods: {
    // setColor(color) {
    //   this.color = color;
    //   this.$emit("setTheColor", color);
    // },

  setColor(color) {
  if (this.selectedColor === color) {
    // Toggle off if clicking on the same color again
    this.selectedColor = '';
  } else {
    // Toggle on for the clicked color and toggle off for others
    this.selectedColor = color;
  }

  // Emit the selected color to the parent component if needed
  this.$emit("setTheColor", this.selectedColor);

  // Update glowEffects based on selectedColor
  Object.keys(this.glowEffects).forEach(key => {
    this.glowEffects[key] = (key === this.selectedColor);
  });

  },

    next() {
      if (this.showPart === 1) {
        this.showArrow = false;

        // let audio = this.$refs.audioElement;
        //   audio.play();
        // let soundTimer = setInterval(()=> {
        //   audio.pause();
        //   audio.currentTime = 0;
        //   }, 3000);

        //   let timer =  setTimeout(()=> {
        //   this.showArrow = true;
        //   clearInterval(soundTimer);
        // }, 29000);

        let timer = setTimeout(() => {
          this.showArrow = true;
        }, 18000);
      }
      this.showPart++;
      if (this.showPart === 3) {
        this.$emit("next");
        // clearTimeout(timer);
      }
    },
  },
};
</script>

<style scoped>
body {
  width: 100vw;
  height: 100vh;
}
#intro {
  position: absolute;
  top: 0%;
  overflow: hidden;
}

/*  */

.imposter {
  position: absolute;
  top: 25%;
  left: 50%;
  transform: translateX(-50%);
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
  width: 35px;
  height: 80px;
  background: var(--dark-color);
  position: absolute;
  top: 40px;
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

/*  */

.player {
  position: absolute;
  top: 20%;
  left: 50%;
  transform: translateX(-50%);
  width: 80vw;
  height: 35vh;
}

.text {
  font-family: bn pixeliom;
  color: white;
  position: absolute;
  top: 8%;
  left: 50%;
  transform: translateX(-50%);
  font-size: 3.3vh;
  width: 95%;
  direction: rtl;
}

.color-container {
  position: absolute;
  bottom: 20%;
  left: 50%;
  transform: translateX(-50%);
  background-color: #293031;
  border: 2px solid #101818;
  width: 70vw;
  height: 20vh;
}
.pick-color {
  width: 100vw;
  height: 100vh;
}

.color {
  width: 100%;
}

.arrow {
  width: 20vw;
  height: 9vh;
  position: absolute;
  bottom: 5%;
  left: 50%;
  transform: translateX(-50%);
  animation: float 2s linear infinite;
}

@keyframes float {
  0% {
    bottom: 5%;
  }
  50% {
    bottom: 8%;
  }
  100% {
    bottom: 5%;
  }
}

@-webkit-keyframes float {
  0% {
    bottom: 5%;
  }
  50% {
    bottom: 8%;
  }
  100% {
    bottom: 5%;
  }
}

.instructions {
  width: 100vw;
  height: 100vh;
}

.finale-exe {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  width: 100%;
  height: 70%;
  /* left: 30%; */
  bottom: 20%;
  border-radius: 50px;
  box-shadow: 0 15px 20px -20px rgba(0, 0, 0, 0.4);
  padding: 0 2rem;
  direction: rtl;
  font-size: 8vw;
  font-family: bn pixeliom;
}

.type-writer > p {
  animation: typeWrite 2s var(--delay) steps(44) normal both,
    blink 400ms var(--delay) steps(45) 6;
}

@keyframes typeWrite {
  from {
    width: 0%;
  }

  to {
    width: var(--width);
  }
}

.finale-exe > p {
  display: block;
  width: fit-content;
  white-space: nowrap;
  text-align: right;
  overflow: hidden;
  border-left: solid 3px transparent;
  max-width: fit-content;
  margin: 4%;
}

@keyframes blink {
  to {
    border-left-color: black;
  }

  from {
    border-left-color: transparent;
  }
}

.text-writer {
  text-align: right;
}
</style>