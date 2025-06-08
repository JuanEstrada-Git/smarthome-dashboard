<template>
  <main class="dashboard">
    <h2>Welcome to Your New Smart Home!!</h2>
    <p>Here’s your Smart Home at a glance.</p>
    <SmartHomeCard />
    <LightsCard />
    <SecurityCard />
    <ThermostatCard />
  </main>
</template>

<script>
import SmartHomeCard from './SmartHomeCard.vue'
import LightsCard from './LightsCard.vue'
import SecurityCard from './SecurityCard.vue'
import ThermostatCard from './ThermostatCard.vue'

export default {
  name: 'SmartDashboard',
  components: {
    SmartHomeCard,
    LightsCard,
    SecurityCard,
    ThermostatCard
  }
}
</script>

<style scoped>
.dashboard {
  margin: 3rem auto;
  max-width: 400px;
  background: #23252b;
  border-radius: 1.5rem;
  padding: 2rem;
  color: #eee;
  box-shadow: 0 6px 32px #0002;
}
</style>
