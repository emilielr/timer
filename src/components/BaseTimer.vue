<template>
    <div class="base-timer">
        <svg class="base-timer__svg" 
            viewBox="0 0 100 100" 
            xmlns="http://www.w3.org/2000/svg"
        >
        <g class="base-timer__circle">
            <circle
                class="base-timer__path-elapsed"
                cx="50"
                cy="50"
                r="46.5"
            ></circle>
            <path 
                :stroke-dasharray="circleDasharray"
                class="base-timer__path-remaining"
                d="
                    M 50, 50
                    m -45, 0
                    a 45,45 0 1,0 90,0
                    a 45,45 0 1,0 -90,0
                "
            ></path>
        </g>
        </svg>

        <span class="base-timer__label">
            {{ timeLeft }}
        </span>

    </div>
</template>

<script>
const FULL_DASH_ARRAY = 283;
export default {
    props: {
        timeLeft: {
            type: String,
            required: true
        }
    },

    computed: {
        circleDasharray() {
            return `${(this.timeFraction * FULL_DASH_ARRAY).toFixed(0)} 283`;
        },
        timeFraction() {
            return this.timeLeft / this.timeLimit;
        }
    },
}
</script>

<style scoped>
/* Sets the containters height and width*/
.base-timer {
    position: relative;
    width: 300px;
    height: 300px;
}

/* Removes SVG styling that would hide the time label */
.base-timer__circle {
    fill: none;
    stroke: none;
}

/* The SVG path that displays the timer's progress */
.base-timer__path-elapsed {
    stroke-width: 7px;
    stroke:grey;
}

.base-timer__label {
    position: absolute;
    width: 300px;
    height: 300px;
    top: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 65px;
}

.base-timer__path-remaining {
    stroke-width: 7px;
    stroke-linecap: round;
    transform: rotate(90deg);
    transform-origin: center;
    transition: 1s linear all;
    stroke: rgb(65, 184, 131);
}

.base-timer__svg {
    transform: scaleX(-1);
}
</style>