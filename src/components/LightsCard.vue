<template>
  <div class="card">
    <div class="icon-row">
      <span class="bulb" :class="{ on: lightOn }">
        <svg width="38" height="38" viewBox="0 0 38 38">
          <ellipse cx="19" cy="17" rx="10" ry="12" :fill="lightOn ? '#ffec8a' : '#505869'" />
          <rect x="15" y="28" width="8" height="7" rx="2" :fill="lightOn ? '#ffe572' : '#4d525a'" />
        </svg>
      </span>
      <span class="status-dot" :class="{ on: lightOn }"></span>
    </div>
    <h3>Living Room Lights</h3>
    <button @click="toggleLight">{{ lightOn ? 'Turn Off' : 'Turn On' }}</button>
    <p class="status">
      Status: <b :style="{ color: lightOn ? '#ffe572' : '#cfd8ff' }">{{ lightOn ? 'On' : 'Off' }}</b>
    </p>
  </div>
</template>



<script>

import axios from 'axios';

export default {
  data() {
    return {
      lightOn: null
    }
  },
  mounted() {
    axios.get('http://localhost:3001/api/devices')
      .then(res => {
        const device = res.data.find(d => d.id === 1);
        this.lightOn = device.status === 'on';
      })
      .catch(err => {
        console.error("Error fetching device state:", err);
      });
  },
  methods: {
    toggleLight() {
      const newStatus = this.lightOn ? 'off' : 'on';
      axios.post('http://localhost:3001/api/devices/1', { status: newStatus })
        .then(res => {
          this.lightOn = res.data.status === 'on';
        })
        .catch(err => {
          console.error("Error updating device state:", err);
        });
    }
  }
}
</script>


<style scoped>

.card {
  background: var(--color-bg-card);
  border-radius: 22px;
  padding: 2.3rem 1.7rem 1.7rem 1.7rem;
  margin: 1rem 0;
  color: var(--color-text-main);
  box-shadow: 0 6px 40px var(--color-shadow-blue), 0 0 0 4px var(--color-shadow-glow);
  max-width: 350px;
  text-align: center;
  backdrop-filter: blur(4px);
  border: 2px solid #3a4151;
  transition: box-shadow 0.2s, background 0.2s, color 0.2s;
}
.status-dot {
  background: var(--color-dot-off);
}
.status-dot.on {
  background: var(--color-status-info);
  box-shadow: 0 0 10px 2px var(--color-status-info);
  animation: pulseDot 1.2s infinite alternate;
}
.status {
  color: var(--color-status-info);
}
button {
  background: var(--color-button-bg);
  color: var(--color-bg-card-alt);
}
button:hover {
  background: var(--color-button-hover);
}

.icon-row {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.8rem;
  margin-bottom: 0.6rem;
}
.bulb svg {
  filter: drop-shadow(0 2px 8px #ffe57277);
  transition: filter 0.3s;
}
.bulb.on svg ellipse {
  filter: drop-shadow(0 0 16px #ffec8a99);
}

@keyframes pulseDot {
  from { box-shadow: 0 0 10px 2px #ffe57299; }
  to   { box-shadow: 0 0 18px 8px #ffe57244; }
}



</style>
