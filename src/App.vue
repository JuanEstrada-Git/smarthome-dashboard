<template>
  <div id="app">
    <!-- Only ONE header (the blue one with sticky, awesome style) -->
    <header class="main-header">
      <h1>Smart Home Dashboard</h1>
      <button class="theme-switch" @click="toggleTheme">
        <span v-if="darkMode">🌞</span>
        <span v-else>🌙</span>
      </button>
    </header>
    <!-- Your Dashboard (everything else) -->
    <SmartDashboard />
  </div>
</template>

<script>
import SmartDashboard from './components/Dashboard.vue'

export default {
  name: 'App',
  components: {
    SmartDashboard
  },
  data() {
    return {
      darkMode: true
    }
  },
  methods: {
    toggleTheme() {
      this.darkMode = !this.darkMode
      localStorage.setItem('darkMode', this.darkMode)
      document.body.className = this.darkMode ? 'theme-dark' : 'theme-light'
    }
  },
  mounted() {
    const stored = localStorage.getItem('darkMode')
    if (stored !== null) {
      this.darkMode = stored === 'true'
    }
    document.body.className = this.darkMode ? 'theme-dark' : 'theme-light'
  }
}
</script>

<style>
/* Styles for sticky header and theme toggle */
.main-header {
  position: sticky;
  top: 0;
  z-index: 100;
  background: linear-gradient(90deg, #232f45 0%, #2982f7 100%);
  color: #ffda79;
  padding: 1.2rem 0 1.1rem 0;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 2.6rem;
  font-family: 'Montserrat', 'Avenir', Helvetica, Arial, sans-serif;
  box-shadow: 0 3px 14px #00338818, 0 1px 0 #ffda79;
  margin-bottom: 2rem;
}
.main-header h1 {
  margin: 0;
  flex: 1;
  font-weight: 800;
  letter-spacing: 0.02em;
  text-align: center;
}
.theme-switch {
  margin-left: 1.5rem;
  background: #fff;
  border: 2px solid #e0e0e0;
  border-radius: 12px;
  font-size: 2.2rem;
  padding: 0.2em 0.38em;
  box-shadow: 0 2px 16px #0033880a;
  cursor: pointer;
  transition: box-shadow 0.2s;
  outline: none;
}
.theme-switch:hover { box-shadow: 0 2px 18px #00338822; }
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0;
}

:root {
  --color-bg-card: #35404f;
  --color-bg-card-alt: #23252b;
  --color-text-main: #fffbe7;
  --color-text-heading: #fffbe7;
  --color-text-muted: #b8c4d7;
  --color-status-ok: #00ff87;
  --color-status-warn: #ff6464;
  --color-status-info: #ffe572;
  --color-dot-off: #b8c4d7;
  --color-shadow-glow: #ffe57244;
  --color-shadow-blue: #abc9ff22;
  --color-button-bg: #ffe572;
  --color-button-hover: #ffd13e;
}

body.theme-light {
  --color-bg-card: #fff;
  --color-bg-card-alt: #f7f8fa;
  --color-text-main: #23252b;
  --color-text-heading: #23252b;
  --color-text-muted: #7885a1;
  --color-status-ok: #21b481;
  --color-status-warn: #d23434;
  --color-status-info: #e4b700;
  --color-dot-off: #a2acc9;
  --color-shadow-glow: #ffe57218;
  --color-shadow-blue: #abc9ff18;
  --color-button-bg: #ffe572;
  --color-button-hover: #ffd13e;
}

body.theme-dark { background: #181a20; color: #eee; }
body.theme-light { background: #f8f9fc; color: #181a20; }

/* Cards, etc */
body.theme-dark .card, body.theme-dark .smart-home-card { background: #35404f; color: #eee; }
body.theme-light .card, body.theme-light .smart-home-card {
  background: #fff; color: #222;
  box-shadow: 0 8px 44px #abc9ff22, 0 0 0 4px #7bb6ff30, 0 0 18px 6px #7bb6ff44;
}
</style>
