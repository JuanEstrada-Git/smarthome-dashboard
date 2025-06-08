<template>
  <div class="card smart-home-card">
    <div class="status-row">
      <span class="energy-ring" :style="{ boxShadow: ringShadow }"></span>
      <div class="stats">
        <div>Devices: <b>{{ devices.length }}</b></div>
        <div>Power: <b>{{ totalPower }}W</b></div>
        <div :class="['status', securityStatus.toLowerCase()]">{{ securityStatus }}</div>
      </div>
    </div>
    <div class="modes">
      <button
        v-for="mode in modes"
        :key="mode"
        :class="{ active: mode === currentMode }"
        @click="setMode(mode)"
      >
        {{ mode }}
      </button>
    </div>
    <div class="devices">
      <div v-for="dev in devices" :key="dev.id" class="device">
        <span :class="['dot', dev.online ? 'on' : 'off']"></span>
        {{ dev.name }} –
        <span
          class="device-battery"
          :data-warn="dev.battery < 20"
        >
          {{ dev.battery }}%
        </span>
        <button v-if="!dev.online" @click="rebootDevice(dev.id)">Reboot</button>
      </div>
    </div>
    <div class="events">
      <div v-for="event in lastEvents" :key="event.time" class="event">
        <span class="event-icon">{{ event.icon }}</span> {{ event.text }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SmartHomeCard',
  data() {
    return {
      currentMode: 'Home',
      modes: ['Home', 'Away', 'Night'],
      securityStatus: 'Armed',
      devices: [
        { id: 1, name: 'Living Room Light', online: true, battery: 100 },
        { id: 2, name: 'Front Door Sensor', online: false, battery: 12 },
        { id: 3, name: 'Thermostat', online: true, battery: 79 },
      ],
      totalPower: 73,
      lastEvents: [
        { icon: '🚪', text: 'Front door opened', time: '19:42' },
        { icon: '🔥', text: 'Temp rising in kitchen', time: '19:38' },
        { icon: '🔋', text: 'Sensor battery low', time: '19:20' },
      ],
    };
  },
  computed: {
    ringShadow() {
      const power = Math.min(this.totalPower, 300) / 7;
      return `0 0 ${7 + power}px 2.5px var(--color-shadow-glow)`;
    },
  },
  methods: {
    setMode(mode) {
      this.currentMode = mode;
    },
    rebootDevice(id) {
      this.devices = this.devices.map(d =>
        d.id === id ? { ...d, online: true } : d
      );
      this.lastEvents.unshift({
        icon: '🔄',
        text: `Rebooted device ${id}`,
        time: new Date().toLocaleTimeString(),
      });
    },
  },
};
</script>

<style scoped>
.card.smart-home-card {
  background: var(--color-bg-card);
  border-radius: 22px;
  padding: 2.1rem 1.6rem 1.3rem 1.6rem;
  color: var(--color-text-main);
  box-shadow:
    0 6px 40px var(--color-shadow-blue),
    0 0 0 3px var(--color-shadow-glow);
  border: 2px solid #3a4151;
  max-width: 380px;
  margin: 1.2rem auto;
  backdrop-filter: blur(4px);
  text-align: left;
  position: relative;
  transition: box-shadow 0.18s, background 0.2s, color 0.2s;
}
.status-row {
  display: flex;
  align-items: center;
  gap: 1.2rem;
  margin-bottom: 0.4rem;
}
.energy-ring {
  width: 32px; height: 32px; border-radius: 50%;
  background: #181a20;
  margin-right: 0.3rem;
  display: inline-block;
  border: 3px solid var(--color-status-info);
  box-shadow: 0 0 10px 2px var(--color-shadow-glow);
  transition: box-shadow 0.24s;
}
.stats { font-size: 1.03rem; }
.status { font-weight: bold; }
.status.armed { color: var(--color-status-ok); }
.status.disarmed { color: var(--color-status-warn); }
.modes {
  margin: 1.05rem 0 0.7rem 0;
  display: flex; gap: 0.75rem; justify-content: flex-start;
}
.modes button {
  background: var(--color-bg-card-alt);
  color: var(--color-status-info);
  border: 2px solid var(--color-status-info);
  border-radius: 16px;
  font-weight: bold;
  padding: 0.48rem 1.09rem;
  cursor: pointer;
  font-size: 1.04rem;
  transition: background 0.18s, color 0.18s;
}
.modes button.active,
.modes button:hover {
  background: var(--color-status-info);
  color: var(--color-bg-card-alt);
}
.devices {
  margin: 0.6rem 0 0.7rem 0;
  border-top: 1px solid #cfcfcf2c;
  border-bottom: 1px solid #cfcfcf2c;
  padding: 0.7rem 0;
}
.device { margin: 0.32rem 0; }
.dot {
  width: 10px; height: 10px; border-radius: 50%; display: inline-block;
  margin-right: 8px;
  background: var(--color-dot-off);
}
.dot.on { background: var(--color-status-info); }
.device button {
  margin-left: 0.7rem;
  font-size: 0.89em;
  background: var(--color-status-info);
  color: var(--color-bg-card-alt);
  padding: 0.13em 0.82em;
  border: none;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
  box-shadow: 0 1px 3px #fff4;
  transition: background 0.19s;
}
.device button:hover { background: var(--color-button-hover); }
.events { margin: 0.82rem 0 0 0; }
.event { font-size: 0.99rem; margin-top: 0.44rem; opacity: 0.87; }
.event-icon { margin-right: 7px; }

/* HIGH-CONTRAST BATTERY PERCENTAGE FOR LIGHT MODE */
body.theme-light .smart-home-card .device-battery {
  font-weight: 700;
  color: #a17a00 !important; /* Gold for info */
  text-shadow:
    0 0 2px #fff,
    0 1px 8px #ffd13e,
    0 0.5px 1px #444;
}
body.theme-light .smart-home-card .device-battery[data-warn="true"] {
  color: #b40019 !important; /* Deep red for warning */
  text-shadow:
    0 0 2px #fff,
    0 1px 8px #ff6868,
    0 0.5px 1px #333;
}
/* SOFTER BATTERY CONTRAST FOR DARK MODE */
body.theme-dark .smart-home-card .device-battery {
  font-weight: 700;
  color: #ffe572 !important;
  text-shadow:
    0 0 2px #23252b,
    0 1px 4px #0005;
}
body.theme-dark .smart-home-card .device-battery[data-warn="true"] {
  color: #ff6464 !important;
  text-shadow:
    0 0 2px #23252b,
    0 1px 4px #0005;
}
</style>
