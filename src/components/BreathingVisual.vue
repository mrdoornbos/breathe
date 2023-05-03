<template>
  <div class="app" :class="theme">
    <h1 class="title">JUST BREATHE</h1>
    <svg class="circle-container" :width="width" :height="height">
      <circle
        class="breathing-circle"
        :cx="width / 2"
        :cy="height / 2"
        :r="currentRadius"
        :style="circleStyle"
      />
    </svg>
    <div class="config">
      <label for="breath-cycle">Breath cycle (seconds):</label>
      <select v-model="breathCycle" id="breath-cycle">
        <option v-for="n in 8" :value="n + 2" :key="n">{{ n + 2 }}</option>
      </select>
      <button @click="toggleTheme">Toggle theme</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      width: 300,
      height: 300,
      baseRadius: 100,
      currentRadius: 100,
      breathCycle: 6,
      animationFrame: null,
      colors: [
        '#FFC107',
        '#9C27B0',
        '#3F51B5',
        '#03A9F4',
        '#009688',
        '#8BC34A',
        '#FF5722',
        '#795548',
      ],
      theme: 'dark',
    };
  },
  computed: {
    circleStyle() {
      return {
        fill: this.colors[this.breathCycle - 3],
      };
    },
  },
  watch: {
    breathCycle(newCycle) {
      this.setBreathingAnimation(newCycle);
    },
  },
  mounted() {
    this.setBreathingAnimation(this.breathCycle);
  },
  beforeUnmount() {
    if (this.animationFrame) {
      cancelAnimationFrame(this.animationFrame);
    }
  },
  methods: {
    setBreathingAnimation(cycle) {
      if (this.animationFrame) {
        cancelAnimationFrame(this.animationFrame);
      }
      const breathe = (timestamp) => {
        const elapsed = (timestamp % (cycle * 2000)) / 1000;
        const progress = elapsed / (cycle * 2);
        const scaleFactor = 1 + 0.5 * Math.sin(progress * 2 * Math.PI);
        this.currentRadius = this.baseRadius * scaleFactor;
        this.animationFrame = requestAnimationFrame(breathe);
      };
      this.animationFrame = requestAnimationFrame(breathe);
    },
    toggleTheme() {
      this.theme = this.theme === 'light' ? 'dark' : 'light';
    },
  },
};
</script>

<style scoped>
@font-face {
  font-family: "Commodore 64";
  src: url("~@/assets/fonts/commodore-64-pixelized.ttf") format("truetype");
}

.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  transition: background-color 0.5s ease;
}

.light {
  background-color: #f5f5f5;
}

.dark {
  background-color: #212121;
}

.title {
  font-family: "Commodore 64", monospace;
  font-size: 2rem;
  margin-bottom: 20px;
  transition: color 0.5s ease;
}

.light .title {
  color: #212121;
}

.dark .title {
  color: #f5f5f5;
}

.circle-container {
  margin-bottom: 20px;
}

.config {
  display: flex;
  align-items: center;
  gap: 8px;
  transition: color 0.5s ease;
}

.light .config {
  color: #212121;
}

.dark .config {
  color: #f5f5f5;
}

button {
  background-color: transparent;
  border: 1px solid currentColor;
  border-radius: 4px;
  color: inherit;
  cursor: pointer;
  font-size: 1rem;
  padding: 6px 12px;
  transition: background-color 0.3s ease, color 0.3s ease;
}

button:hover {
  background-color: currentColor;
  color: #f5f5f5;
}
</style>

