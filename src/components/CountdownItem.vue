<template>
    <div class="timer">
        <svg class="timer__svg" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
            <g class="timer__circle">
                <circle class="timer__path-elapsed" cx="50" cy="50" r="45"></circle>
                <path :stroke-dasharray="circleDasharray" class="timer__path-remaining" :class="remainingPathColor" d="
              M 50, 50
              m -45, 0
              a 45,45 0 1,0 90,0
              a 45,45 0 1,0 -90,0
            "></path>
            </g>
        </svg>
        <span class="timer__label">{{ formattedTimeLeft }}</span>
        <div class="timer__buttons">
            <div class="timer__button" v-if="!running" @click="startTimer()">
                <svg version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"
                    x="0px" y="0px" viewBox="0 0 320.001 320.001" style="enable-background:new 0 0 320.001 320.001;"
                    xml:space="preserve">
                    <path d="M295.84,146.049l-256-144c-4.96-2.784-11.008-2.72-15.904,0.128C19.008,5.057,16,10.305,16,16.001v288
          c0,5.696,3.008,10.944,7.936,13.824c2.496,1.44,5.28,2.176,8.064,2.176c2.688,0,5.408-0.672,7.84-2.048l256-144
          c5.024-2.848,8.16-8.16,8.16-13.952S300.864,148.897,295.84,146.049z" />
                </svg>
            </div>
            <div class="timer__button" v-if="running" @click="pauseTimer()">
                <svg version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"
                    x="0px" y="0px" width="519.479px" height="519.479px" viewBox="0 0 519.479 519.479"
                    style="enable-background:new 0 0 519.479 519.479;" xml:space="preserve">
                    <g>
                        <g>
                            <path d="M193.441,0h-75.484c-16.897,0-30.6,13.703-30.6,30.6v458.277c0,16.898,13.703,30.602,30.6,30.602h75.484
                  c16.897,0,30.6-13.703,30.6-30.602V30.6C224.042,13.703,210.339,0,193.441,0z" />
                            <path d="M401.521,0h-75.484c-16.896,0-30.6,13.703-30.6,30.6v458.277c0,16.898,13.703,30.602,30.6,30.602h75.484
                  c16.896,0,30.6-13.703,30.6-30.602V30.6C432.121,13.703,418.418,0,401.521,0z" />
                        </g>
                    </g>
                </svg>
            </div>
            <div class="timer__button" @click="resumeTimer()">
                <svg fill="none" height="512" viewBox="0 0 24 24" width="512" xmlns="http://www.w3.org/2000/svg">
                    <path
                        d="m6 3.25c-1.51878 0-2.75 1.23122-2.75 2.75v12c0 1.5188 1.23122 2.75 2.75 2.75h12c1.5188 0 2.75-1.2312 2.75-2.75v-12c0-1.51878-1.2312-2.75-2.75-2.75z" />
                </svg>
            </div>
        </div>
    </div>
</template>
  
<script>
const FULL_DASH_ARRAY = 283;
const WARNING_THRESHOLD = 30;
const ALERT_THRESHOLD = 15;

const COLOR_CODES = {
    info: {
        color: "green"
    },
    warning: {
        color: "orange",
        threshold: WARNING_THRESHOLD
    },
    alert: {
        color: "red",
        threshold: ALERT_THRESHOLD
    }
};

const TIME_LIMIT = 900; // Duration in seconds.

export default {
    data() {
        return {
            timePassed: 0,
            timerInterval: null,
            running: false
        };
    },

    computed: {
        circleDasharray() {
            return `${(this.timeFraction * FULL_DASH_ARRAY).toFixed(0)} 283`;
        },

        formattedTimeLeft() {
            const timeLeft = this.timeLeft;
            const minutes = Math.floor(timeLeft / 60);
            let seconds = timeLeft % 60;

            if (seconds < 10) {
                seconds = `0${seconds}`;
            }

            return `${minutes}:${seconds}`;
        },

        timeLeft() {
            return TIME_LIMIT - this.timePassed;
        },

        timeFraction() {
            const rawTimeFraction = this.timeLeft / TIME_LIMIT;
            return rawTimeFraction - (1 / TIME_LIMIT) * (1 - rawTimeFraction);
        },

        remainingPathColor() {
            const { alert, warning, info } = COLOR_CODES;

            if (this.timeLeft <= alert.threshold) {
                return alert.color;
            } else if (this.timeLeft <= warning.threshold) {
                return warning.color;
            } else {
                return info.color;
            }
        }
    },

    watch: {
        timeLeft(newValue) {
            if (newValue === 0) {
                this.onTimesUp();
            }
        }
    },

    mounted() {
        // this.startTimer();
    },

    methods: {
        onTimesUp() {
            clearInterval(this.timerInterval);
        },

        startTimer() {
            this.running = true;
            this.timerInterval = setInterval(() => (this.timePassed += 1), 1000);
        },

        pauseTimer() {
            this.running = false;
            clearInterval(this.timerInterval);
        },

        resumeTimer() {
            this.pauseTimer();
            this.timePassed = 0;
        },
    }
};
</script>
  
<style scoped>
.timer {
    position: relative;
    width: 300px;
    height: 300px;
    z-index: 100;
    margin-top: 10rem;
}

.timer__svg {
    transform: scaleX(-1);
}

.timer__circle {
    fill: none;
    stroke: none;
}

.timer__path-elapsed {
    stroke-width: 7px;
    stroke: grey;
}

.timer__path-remaining {
    stroke-width: 7px;
    stroke-linecap: round;
    transform: rotate(90deg);
    transform-origin: center;
    transition: 1s linear all;
    fill-rule: nonzero;
    stroke: currentColor;
}

.timer__path-remaining.green {
    color: rgb(65, 184, 131);
}

.timer__path-remaining.orange {
    color: orange;
}

.timer__path-remaining.red {
    color: red;
}

.timer__label {
    position: absolute;
    width: 300px;
    height: 300px;
    top: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 48px;
    color: white;
}

.timer__buttons {
    text-align: center;
}

.timer__button {
    background-color: rgb(65, 184, 131);
    margin: 0.5rem;
    border-radius: 50%;
    height: 3rem;
    width: 3rem;
    position: relative;
    overflow: hidden;
    display: inline-block;
}

.random {
    transform: rotate(360deg);
    transition: transform 0.5s;
}

.random:active {
    transform: rotate(0deg);
    transition: 0s;
}

.timer__button:hover {
    cursor: pointer;
}

.timer__button svg {
    width: 50%;
    fill: white;
    top: 50%;
    left: 50%;
    position: absolute;
    transform: translate(-50%, -50%);
}
</style>