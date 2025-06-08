<template>
  <header class="header">
    <h1>Smart Home Dashboard</h1>
  </header>
</template>

<script>
export default {
  name: 'AppHeader'
}
</script>

<style scoped>
.header {
  position: sticky;
  top: 0;
  z-index: 1000;
  background: linear-gradient(90deg, #23252b 0%, #246afd 100%);
  color: #ffda79;
  text-align: center;
  font-size: 2.6rem;
  font-family: 'Inter', 'Segoe UI', sans-serif;
  letter-spacing: 1.5px;
  font-weight: 700;
  padding: 1.3rem 0 1rem 0;
  box-shadow: 0 4px 24px 0 #246afd88, 0 2px 0 #ffda79;
  border-bottom: 2px solid #ffda79;
  transition: box-shadow 0.2s;
  /* subtle sticky shadow on scroll */
}
.header h1 {
  margin: 0;
  background: linear-gradient(90deg, #ffda79 30%, #fffbe7 70%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  filter: drop-shadow(0 2px 6px #246afd77);
  letter-spacing: 1.5px;
  font-size: 2.4rem;
}
</style>
