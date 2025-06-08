<template>
  <div class="card security-card">
    <div class="icon-row">
      <span class="shield" :class="{ armed }">
        <svg width="28" height="28" viewBox="0 0 24 24">
          <path
            d="M12 3l7 4v4.5c0 4.5-3.2 8.7-7 9.5-3.8-.8-7-5-7-9.5V7l7-4z"
            :fill="armed ? '#ffda79' : '#3e4552'" />
        </svg>
      </span>
      <span class="status-dot" :class="{ armed }"></span>
    </div>
    <h3>Security Status</h3>
    <div class="status-row">
      <span class="sys-status" :class="{ armed }">{{ armed ? 'System Armed (Away)' : 'System Disarmed' }}</span>
    </div>
    <button @click="toggleArm">{{ armed ? 'Disarm System' : 'Arm System' }}</button>
    <hr />
    <div class="doors">
      <strong>Doors/Windows:</strong>
      <ul>
        <li>
          <span class="door-label">Front Door:</span>
          <b :class="{ ok: frontDoorLocked, warn: !frontDoorLocked }">{{ frontDoorLocked ? 'Locked' : 'Unlocked' }}</b>
        </li>
        <li>
          <span class="door-label">Patio Door:</span>
          <b :class="{ ok: patioDoorClosed, warn: !patioDoorClosed }">{{ patioDoorClosed ? 'Closed' : 'Open' }}</b>
        </li>
      </ul>
    </div>
    <div class="last-alert">
      <span>Last Event:</span>
      <b>{{ lastEvent }}</b>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      armed: false,
      frontDoorLocked: true,
      patioDoorClosed: true,
      lastEvent: " No recent incidents"
    }
  },
  methods: {
    toggleArm() {
      this.armed = !this.armed
      this.lastEvent = this.armed ? " System armed" : " System disarmed"
    }
  }
}
</script>

<style scoped>
.security-card {
  background: rgba(53, 64, 79, 0.95);
  border-radius: 22px;
  padding: 2.2rem 1.7rem 1.4rem 1.7rem;
  color: #fffbe7;
  box-shadow: 0 8px 44px #246afd22, 0 0 0 4px #ffda7930;
  border: 2px solid #313a52;
  text-align: center;
  max-width: 350px;
  margin: 1rem 0;
  backdrop-filter: blur(6px);
}
.icon-row {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.7rem;
  margin-bottom: 0.65rem;
}
.shield svg {
  filter: drop-shadow(0 2px 8px #ffda7980);
  transition: filter 0.3s;
}
.shield.armed svg {
  filter: drop-shadow(0 0 16px #ffda79ee);
}
.status-dot {
  width: 14px; height: 14px;
  border-radius: 50%;
  background: #e55;
  border: 2px solid #23252b;
  box-shadow: 0 2px 6px #23252b44;
  display: inline-block;
  transition: background 0.2s;
}
.status-dot.armed {
  background: #ffe572;
  box-shadow: 0 0 12px 2px #ffe57299, 0 2px 6px #23252b44;
  animation: pulseDot 1.2s infinite alternate;
}
@keyframes pulseDot {
  from { box-shadow: 0 0 10px 2px #ffe57299; }
  to   { box-shadow: 0 0 18px 8px #ffe57244; }
}
.sys-status {
  font-size: 1.13rem;
  font-weight: 600;
  color: #eee;
  letter-spacing: 0.2px;
  margin-bottom: 0.6rem;
  display: inline-block;
  transition: color 0.3s;
}
.sys-status.armed { color: #ffe572; }
button {
  margin-top: 8px;
  padding: 0.5rem 1.2rem;
  background: #ffda79;
  color: #181a20;
  border: none;
  border-radius: 13px;
  font-weight: bold;
  font-size: 1.08rem;
  cursor: pointer;
  box-shadow: 0 2px 8px #fff4;
  transition: background 0.2s, box-shadow 0.2s;
}
button:hover { background: #ffd13e; box-shadow: 0 4px 12px #ffd13e44; }
hr {
  border: none;
  border-top: 2px solid #fff4;
  margin: 1.1rem 0 0.7rem 0;
}
.doors {
  margin-top: 0.1rem;
  font-size: 1.07rem;
  text-align: left;
}
.doors ul { padding-left: 1.3em; margin: 0.5em 0 0 0; }
.door-label { color: #b8c4d7; margin-right: 0.2em; }
.ok { color: #ffe572; }
.warn { color: #e55; }
.last-alert {
  margin-top: 1.1rem;
  font-size: 0.98em;
  opacity: 0.8;
  text-align: left;
}
</style>
