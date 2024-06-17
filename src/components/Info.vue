<template>
  <div id="info">
    <div class="space-right">
          <img v-if="type === '0'" class="baby meteor" src="../../src/assets/pictures/meteor.svg" alt="meteor"/>
    <img v-else-if="type === '1'" class="teenage meteor" src="../../src/assets/pictures/meteor.svg" alt="meteor"/>
    <img  v-else-if="type === '2'" class="adult meteor" src="../../src/assets/pictures/meteor.svg" alt="meteor"/>
<p class="headline">{{arr[type].Headline}}</p>
    <p class="information">
      {{arr[type].Text}}
    </p>
    <button :style="{'--chosen-color': theColor, color: theColor === 'yellow' ? 'black' : 'white'}" @click="back" class="continue-btn">בואי נמשיך ללמוד!</button>
    </div>
    
  </div>
</template>

<script>
export default {
  name: "info",
  components: {},
  props: ["theColor", "type"],
  data() {
    return {
      show: false,
      arr: [
        {
            "Headline": "אסטרואיד בייבי",
            "Text": "אסטרואיד ממש קטן והורס רק שכונות מקומיות.",
        },
        {
            "Headline": "אסטרואיד נער מתבגר",
            "Text": "אסטרואיד שהוא די חרא כי הוא בכוונה מנסה להפציץ מקומות שהוא לא אוהב.",
        },
        {
            "Headline": "אסטרואיד בוגר",
            "Text": "אסטרואיד ממש גדול שיכול להשמיד את כל כדור הארץ.",
        },
      ],
    };
  },
  methods: {
    back() {
      this.$emit('back');
    },
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

#Info{
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
  margin-top: 60%;
  text-align: center;
}

.continue-btn {

  border-radius: 4vw;
    background-color: var(--chosen-color);
    padding: 2vw;
    color: white;
    font-family: BN Pinky;
    font-size: 5vh;
    position: absolute;
    top: 100%;
    left: 50%;
    transform: translateX(-50%);
}
.information {
    text-align: center;
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
</style>
