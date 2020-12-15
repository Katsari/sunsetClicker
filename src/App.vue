<template>
  <div class="flex-container">
    <section id="sun">
      <button
        type="button"
        @keydown.enter.prevent
        @keydown.space.prevent
        @click="updateCount"
        v-if="secs > 0"
      >
        &#8594; Click to start &#8592;
      </button>
    </section>

    <section id="game-over" v-if="secs === 0">
      <h2>Game Over!</h2>
      <img src="../src/assets/ravecats.gif" alt="Rave cats" height="250" />
      <button @click="startGame">Restart</button>
    </section>

    <section id="score">
      <h1>
        Score: <strong>{{ numClicks }}</strong
        ><br />
        Clicks/s: <strong>{{ clicksPerSecond }}</strong>
      </h1>
      <h2>
        You have <strong>{{ secs }}</strong> secs left
      </h2>
    </section>

    <div class="ocean">
      <div class="wave"></div>
      <div class="wave"></div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      active: false,
      numClicks: 0,
      secs: 15,
    };
  },
  computed: {
    clicksPerSecond() {
      if (this.secs === 15) {
        return 0;
      }
      return (this.numClicks / (15 - this.secs)).toFixed(2);
    },
  },
  mounted() {
    const self = this;
    setInterval(function () {
      if (self.active) {
        self.secs--;
        if (self.secs === 0) {
          self.active = false;
        }
      }
    }, 1000);
  },
  methods: {
    startGame() {
      this.active = false;
      this.numClicks = 0;
      this.secs = 15;
    },
    updateCount(event) {
      this.numClicks += 1;
      if (this.numClicks === 1) {
        event.target.innerHTML = "";
      }
      if (!this.active) {
        this.secs = 15;
        this.numClicks = 1;
        this.active = true;
      }
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}

*,
*::before,
*::after {
  box-sizing: inherit;
}

html {
  box-sizing: border-box;
}
@media only screen and (max-width: 68.75em) {
  html {
    font-size: 90%;
  }
}

body {
  font-family: "Dosis", sans-serif;
  font-weight: 400;
  line-height: 1.8;
  color: #41344e;
  background: rgb(238, 128, 85);
  background: linear-gradient(
    180deg,
    rgb(130, 101, 168) 0%,
    rgba(208, 97, 67, 1) 42%,
    rgba(249, 208, 126, 1) 100%
  );
  background-size: cover;
  background-repeat: no-repeat;
  height: 100vh;
  overflow: hidden;
}

.flex-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 80vh;
}

section {
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

#sun {
  margin-bottom: 15px;
}
@media only screen and (min-height: 800px) {
  #sun {
    margin-bottom: 50px;
  }
}

#sun button {
  color: #fff;
  font-family: inherit;
  font-size: 30px;
  font-weight: bold;
  text-transform: uppercase;
  border: none;
  width: 300px;
  height: 300px;
  transition: 0.3s;
  border-radius: 50%;
  background: linear-gradient(180deg, #f2bd74, #e9a669, #c9624d, #d25646);
  cursor: pointer;
  -webkit-tap-highlight-color: transparent;
  transition: 0.3s ease-in-out;
}
#sun button:active {
  width: 280px;
  height: 280px;
}
@media only screen and (max-height: 800px) {
  #sun button {
    font-size: 23px;
    width: 250px;
    height: 250px;
  }
  #sun button:active {
    width: 260px;
    height: 260px;
  }
}
#sun button:focus {
  outline: none;
}
#sun button:hover {
  -webkit-box-shadow: 0px 0px 30px #ffa845;
  -moz-box-shadow: 0px 0px 30px #ffa845;
  box-shadow: 0px 0px 30px #ffa845;
  transition: 0.3s ease-in-out;
}

#game-over h2 {
  font-size: 50px;
  text-transform: uppercase;
}
@media only screen and (max-width: 68.75em) {
  #game-over h2 {
    font-size: 35px;
  }
}

#game-over button {
  font-size: 20px;
  font-family: inherit;
  background: #41344e;
  box-shadow: 0px 0px 16px #ffb662;
  border: none;
  transition: all 1s ease-in-out;
  border-image: linear-gradient(#360154, #d442d5) 30;
  color: white;
  padding: 1rem 1.25rem;
  border-radius: 2px;
  margin: 1rem;
  width: 12rem;
  cursor: pointer;
  text-transform: uppercase;
}

#game-over button:focus {
  outline: none;
}

#game-over button:hover,
#game-over button:active {
  box-shadow: 0px 0px 56px #ffb662;
  transition: all 1s ease-in-out;
}

#score h1 {
  font-size: 35px;
  color: #fff;
  font-weight: 300;
}
@media only screen and (max-width: 68.75em) {
  #score h1 {
    font-size: 25px;
  }
}

.ocean {
  position: absolute;
  bottom: 0;
  left: 0;
}

.wave {
  background: url(https://s3-us-west-2.amazonaws.com/s.cdpn.io/85486/wave.svg)
    repeat-x;
  position: absolute;
  top: -198px;
  width: 6400px;
  height: 198px;
  animation: wave 7s cubic-bezier(0.36, 0.45, 0.63, 0.53) infinite;
  transform: translate3d(0, 0, 0);
  filter: opacity(0.3);
  -webkit-filter: opacity(0.3);
}

.wave:nth-of-type(2) {
  top: -175px;
  animation: wave 7s cubic-bezier(0.36, 0.45, 0.63, 0.53) -0.125s infinite,
    swell 7s ease -1.25s infinite;
  opacity: 1;
}

@keyframes wave {
  0% {
    margin-left: 0;
  }
  100% {
    margin-left: -1600px;
  }
}

@keyframes swell {
  0%,
  100% {
    transform: translate3d(0, -25px, 0);
  }
  50% {
    transform: translate3d(0, 5px, 0);
  }
}
</style>
