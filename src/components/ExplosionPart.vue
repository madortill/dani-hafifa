<template>
  <div id="explosion-part">
    <div class="space-right">
<p class="headline">איך מפוצצים אסטרואיד?</p>
    <p class="information">
    תחנת השיגור היא האחראית לחישוב הקורדינאטות שבהן האסטרואיד נמצא בכל רגע נתון.
תפקידכן הוא לכוון את משגר הפצצות לכיוון האסטרואיד לפי הקורדינאטות שקיבלתם מתחנת השיגור.
    </p>

    <img src="../../src/assets/pictures/astroAmongUs.svg" alt="astroAmongUs" class="astroAmongUs" />
    <img src="../../src/assets/pictures/rocket.svg" alt="rocket" class="rocket" />
    <button v-if="showBtn" :style="{'--chosen-color': theColor, color: theColor === 'yellow' ? 'black' : 'white'}" @click="toGame" class="continue-btn">בואי נתרגל</button>
    </div>
    
  </div>
</template>

<script>
export default {
  name: "explosion-part",
  components: {},
  props: ["theColor", "type"],
  data() {
    return {
        showBtn: false,
    };
  },
  methods: {
    toGame() {
      this.$emit('toGame');
    },
  },
  mounted() {
    setTimeout(()=> {
      this.showBtn = true;
    }, 4500);
  },
};
</script>

<style scoped>
body {
  width: 100vw;
  height: 100vh;
  position: absolute;
  top: 0%;
}

#explosion-part{
  background-color: black;
  border: 3px solid white;
  border-radius: 50px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 85vw;
  height: 80%;
   direction: rtl;
   font-size: 5vh;
}

 .headline {
  font-family: bn pixeliom;
  font-size: 4.7vh;
  margin-top: 0%;
}

.continue-btn {

  border-radius: 4vw;
    background-color: var(--chosen-color);
    padding: 2vw;
    color: white;
    font-family: BN Pinky;
    font-size: 5vh;
    position: absolute;
    bottom: -10%;
    left: 25%;
    transform: translateX(-50%);
}
.information {
    width: 80%;
}

.space-right {
  position: absolute; 
    right: 3%;
    left: 3%;
    top:6%;
  }

.meteor {
    position: absolute;
    top:0%;
    animation: spin 4s linear infinite;
}

.baby {
    width: 25%;
    left: 40%;
}

.teenage {
    width: 40%;
    left:30%;
}

.adult {
    width: 60%;
    left:20%;
}

@keyframes spin {
        0% {
            transform: rotate(0deg);
        }
        100% {
            transform: rotate(360deg);
        }
    }
     
    @-webkit-keyframes spin {
        0% {
            transform: rotate(0deg);
        }
        100% {
            transform: rotate(360deg);
        }
    }

    .astroAmongUs {
        position: absolute;
        top: 92%;
    }

    .rocket {
        position: absolute;
        left: 22%;
        animation: launching 4s linear forwards;
    }
    @keyframes launching {
      0% {
        left: 22%;
        top:100%;
      }
      40% {
        left: 5%;
        top: 70%
      }
      70% {
        left: 12%;
        top: 30%;
      }
      100% {
        left: -70%;
        top: 10%
      }
    }
</style>
