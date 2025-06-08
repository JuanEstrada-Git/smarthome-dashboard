<template>
  <div class="card thermostat-card">
    <div class="icon-row">
      <svg class="thermo-dial" width="56" height="56" viewBox="0 0 56 56">
        <circle cx="28" cy="28" r="22" fill="#293340" />
        <circle
          class="arc"
          cx="28" cy="28" r="22"
          :stroke="mode === 'Heating' ? '#ffda79' : '#48e0fa'"
          stroke-width="4"
          fill="none"
          :stroke-dasharray="arcLength"
          stroke-dashoffset="0"
          transform="rotate(-90 28 28)"
        />
        <text x="50%" y="54%" dominant-baseline="middle" text-anchor="middle"
          fill="#fff" font-size="1.1rem" font-weight="bold">
          {{ setpoint }}°
        </text>
      </svg>
    </div>
    <h3>Thermostat</h3>

    <div class="temp-main">
      <span class="current-temp">{{ currentTemp }}°F</span>
      <span class="label">Current</span>
    </div>

    <div class="setpoint-row">
      <button @click="adjustSetpoint(-1)">−</button>
      <span class="setpoint">{{ setpoint }}°F</span>
      <button @click="adjustSetpoint(1)">+</button>
    </div>

    <div class="mode-row">
      <span class="mode-label">Mode: <b :class="mode.toLowerCase()">{{ mode }}</b></span>
      <button class="switch-btn" @click="toggleMode">
        Switch to {{ mode === 'Cooling' ? 'Heating' : 'Cooling' }}
      </button>
    </div>

    <div class="humidity">
      <svg width="18" height="18" style="vertical-align: middle; margin-right: 0.35em;">
        <ellipse cx="9" cy="9" rx="8" ry="7" fill="#80dfff" opacity="0.4"/>
        <ellipse cx="9" cy="9" rx="5" ry="4" fill="#fff" opacity="0.7"/>
      </svg>
      Humidity: <b>{{ humidity }}%</b>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ThermostatCard',
  data() {
    return {
      currentTemp: 72,
      setpoint: 74,
      mode: 'Cooling',
      humidity: 48,
    }
  },
  computed: {
    arcLength() {
      // 2πr * (setpoint-60)/(80-60) for demo: show arc for 60°F-80°F range
      const min = 60, max = 80, r = 22;
      const angle = Math.max(0, Math.min(1, (this.setpoint - min) / (max - min)));
      return `${2 * Math.PI * r * angle} ${2 * Math.PI * r * (1 - angle)}`;
    }
  },
  methods: {
    adjustSetpoint(val) {
      this.setpoint = Math.max(60, Math.min(80, this.setpoint + val));
    },
    toggleMode() {
      this.mode = this.mode === 'Cooling' ? 'Heating' : 'Cooling'
    }
  }
}
</script>

<style scoped>
.thermostat-card {
  background: rgba(53, 64, 79, 0.95);
  border-radius: 22px;
  padding: 2.3rem 1.7rem 1.7rem 1.7rem;
  color: #fffbe7;
  box-shadow: 0 8px 44px #246afd22, 0 0 0 4px #ffda7930;
  max-width: 350px;
  text-align: center;
  border: 2px solid #313a52;
  margin: 1rem 0;
  backdrop-filter: blur(5px);
}
.icon-row {
  display: flex;
  justify-content: center;
  margin-bottom: 0.6rem;
}
.thermo-dial {
  margin-bottom: 0.2rem;
}
.arc {
  transition: stroke 0.4s, stroke-dasharray 0.5s;
}
.temp-main {
  font-size: 2.2rem;
  font-weight: bold;
  margin-bottom: 0.15rem;
}
.label {
  font-size: 1rem;
  color: #ccc;
}
.setpoint-row {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 1rem 0 0.7rem 0;
  gap: 1.1rem;
}
.setpoint {
  font-size: 1.55rem;
  font-weight: bold;
}
button {
  padding: 0.5rem 1.2rem;
  background: #ffda79;
  color: #181a20;
  border: none;
  border-radius: 12px;
  font-weight: bold;
  cursor: pointer;
}
button:hover {
  background: #ffd13e;
}
.mode-row {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1.5rem;
  margin: 1.2rem 0 0.5rem 0;
}
.mode-label {
  font-size: 1.13rem;
}
.mode-label .heating { color: #ffda79; }
.mode-label .cooling { color: #80dfff; }
.switch-btn {
  padding: 0.7rem 2.2rem;
  font-size: 1.09rem;
  border-radius: 15px;
  border: 3px solid #fff;
  background: #ffda79;
  color: #181a20;
  font-weight: bold;
  cursor: pointer;
  box-shadow: 0 2px 8px #0001;
  transition: background 0.2s;
}
.switch-btn:hover {
  background: #ffd13e;
}
.humidity {
  font-size: 1.03rem;
  color: #b8c4d7;
  margin-top: 0.8rem;
  letter-spacing: 0.1px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.humidity b {
  color: #fffbe7;
}
</style>
