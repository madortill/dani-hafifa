<template>
  <div id="finish-page">
    <p class="title">כל הכבוד סיימת את הלומדה בהצלחה!!</p>
    <img class="party" src="../../src/assets/pictures/party.svg" alt="party" />
  </div>
</template>

<script>
export default {
  name: "finish-page",
  components: {},
  data() {
    return {};
  },
  methods: {},
};
</script>

<style scoped>
body {
  width: 100vw;
  height: 100vh;
}

#finish-page {
  width: 100vw;
  height: 100vh;
  position: absolute;
  top: 0%;
  overflow: hidden;
}

.party {
  height: 60vh;
  width: 120vw;
  position: absolute;
  bottom: 0%;
  left: 50%;
  transform: translateX(-50%);
}

.title {
  font-family: bn pixeliom;
  position: absolute;
  top: 15%;
  left: 50%;
  width: 100vw;
  transform: translate(-50%, -50%);
  font-size: 6vh;
  text-align: center;
  direction: rtl;
}
</style>
