<template>
  <div id="app">
    <CountdownItem />
    <div class="timer__aurora"></div>
    <div id='roll' class='roll-button'>
      <button @click="random">
        <svg width="30px" height="30px" viewBox="0 0 512 512" version="1.1" xmlns="http://www.w3.org/2000/svg"
          xmlns:xlink="http://www.w3.org/1999/xlink">
          <g stroke="none" stroke-width="1" fill="none" fill-rule="evenodd">
            <g id="icon" fill="#ffffff" transform="translate(46.976875, 46.976875)">
              <path
                d="M379.689791,38.3564581 L379.689791,379.689791 L38.3564581,379.689791 L38.3564581,38.3564581 L379.689791,38.3564581 Z M337.023125,81.0231247 L81.0231247,81.0231247 L81.0231247,337.023125 L337.023125,337.023125 L337.023125,81.0231247 Z M283.689791,251.689791 C301.362903,251.689791 315.689791,266.016679 315.689791,283.689791 C315.689791,301.362903 301.362903,315.689791 283.689791,315.689791 C266.016679,315.689791 251.689791,301.362903 251.689791,283.689791 C251.689791,266.016679 266.016679,251.689791 283.689791,251.689791 Z M209.023125,177.023125 C226.696237,177.023125 241.023125,191.350013 241.023125,209.023125 C241.023125,226.696237 226.696237,241.023125 209.023125,241.023125 C191.350013,241.023125 177.023125,226.696237 177.023125,209.023125 C177.023125,191.350013 191.350013,177.023125 209.023125,177.023125 Z M134.356458,102.356458 C152.02957,102.356458 166.356458,116.683346 166.356458,134.356458 C166.356458,152.02957 152.02957,166.356458 134.356458,166.356458 C116.683346,166.356458 102.356458,152.02957 102.356458,134.356458 C102.356458,116.683346 116.683346,102.356458 134.356458,102.356458 Z"
                id="Combined-Shape"
                transform="translate(209.023125, 209.023125) rotate(-345.000000) translate(-209.023125, -209.023125) ">
              </path>
            </g>
          </g>
        </svg></button>
    </div>
  </div>
</template>

<script>
import CountdownItem from "./components/CountdownItem";
import { useToast } from "vue-toastification";

export default {
  name: "App",

  components: {
    CountdownItem,
  },

  setup() {
    // Get toast interface
    const toast = useToast();

    // Define developers.
    const developers = [
      'Ruslan',
      'Giada',
      'Valentina',
      'Simone',
      'Andrea',
      'Alessio',
      'Dumitru',
      'Alessandro',
    ];

    // Make it available inside methods
    return { toast, developers }
  },

  methods: {
    showAndRemove() {
      // Get randomly.
      const item = this.developers[Math.floor(Math.random() * this.developers.length)];

      // Remove it.
      const index = this.developers.indexOf(item);

      if (index > -1) {
        this.developers.splice(index, 1);
      }

      return item;
    },
    random() {
      // Get item.
      const item = this.showAndRemove();

      // Since you returned `toast` from setup(), you can access it now
      this.toast.success(item ? item + " 🎉" : 'No more items!', {
        position: 'top-center'
      });
    },
  },

};
</script>

<style>
body {
  background-color: #fff;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}

button {
  position: relative;
  top: 80px;
  padding: 15px 50px;
  color: #fff;
  background-color: #40b883;
  border: none;
  font-size: 20px;
  border-radius: 20px;
  box-shadow: 1px 3px #50514F;
  outline: none;
  transition: .3s;
}

button:hover,
button:active {
  outline: none;
  background: #50514F;
  cursor: pointer;
  transform: translateY(15px);
}

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
}

.timer__aurora {
  position: absolute;
  left: -50%;
  top: -50%;
  width: 200vw;
  height: 200vh;
  animation-name: northern;
  animation-duration: 5s;
  animation-timing-function: linear;
  animation-iteration-count: infinite;
  animation-direction: alternate;
  background: linear-gradient(10deg, #a1d9b4 20%, #32988a 40%, #000e3c 70%);
}

@keyframes northern {
  0% {
    transform: translate(5%, -2%);
  }

  25% {
    transform: translate(10%, 7%);
  }

  40% {
    transform: rotate(-10deg);
  }

  60% {
    transform: translate(7%, -2%);
  }

  85% {
    transform: translate(6%, 3%) rotate(12deg);
  }

  100% {
    transform: none;
  }
}
</style>
