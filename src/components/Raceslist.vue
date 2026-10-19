<template>
  <div>
    <h2>Corridas cadastradas</h2>

    <p v-if="loading">Carregando corridas...</p>

    <p v-else-if="error">
      {{ error }}
    </p>

    <p v-else-if="races.length === 0">
      Nenhuma corrida cadastrada.
    </p>

    <ul v-else>
      <li v-for="race in races" :key="race.id">
        {{ race.name }}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'RacesList',

  data() {
    return {
      races: [],
      loading: false,
      error: null
    }
  },

  async created() {
    await this.fetchRaces()
  },

  methods: {
    async fetchRaces() {
      this.loading = true
      this.error = null

      try {
  const response = await axios.get(
    'http://127.0.0.1:8000/races'
  )

  console.log('Dados recebidos da API:', response.data)

  this.races = response.data
} catch (error) {
  console.error('Erro ao buscar corridas:', error)

  this.error =
    'Não foi possível carregar as corridas. Verifique se a API está funcionando.'
} finally {
  this.loading = false
}
    }
  }
}
</script>

<style scoped>
h2 {
  color: #2c3e50;
}

li {
  margin-bottom: 10px;
}
</style>