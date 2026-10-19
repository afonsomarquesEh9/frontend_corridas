<template>
  <div id="app">
    <header>
      <h1>🏁 Gerenciador de Corridas</h1>
      <p>Organize e acompanhe seus eventos esportivos.</p>
    </header>

    <main>
      <RacesList />
    </main>
  </div>
</template>

<script>
import RacesList from './components/Raceslist.vue'

export default {
  name: 'App',

  components: {
    RacesList
  }
}
</script>

<style>
#app {
  font-family: Arial, sans-serif;
  color: #2c3e50;
  max-width: 900px;
  margin: 40px auto;
  padding: 20px;
}

header {
  text-align: center;
  margin-bottom: 40px;
}

h1 {
  color: #42b983;
}

main {
  padding: 20px;
  background-color: #f5f7fa;
  border-radius: 8px;
}
</style>