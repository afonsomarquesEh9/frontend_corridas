<template>
  <div>
    <h2>Corridas cadastradas</h2>

    <form @submit.prevent="createRace" class="race-form">
  <h3>Cadastrar nova corrida</h3>

  <div class="form-group">
    <label for="name">Nome da corrida</label>

    <input
      id="name"
      v-model="newRace.name"
      type="text"
      placeholder="Ex.: Corrida de São Paulo"
      required
    />
  </div>

  <div class="form-group">
    <label for="location">Localização</label>

    <input
      id="location"
      v-model="newRace.location"
      type="text"
      placeholder="Ex.: São Paulo - SP"
      required
    />
  </div>

  <div class="form-group">
    <label for="distance">Distância (km)</label>

    <input
      id="distance"
      v-model.number="newRace.distance"
      type="number"
      min="0.1"
      step="0.1"
      placeholder="Ex.: 10"
      required
    />
  </div>

  <button type="submit" :disabled="creating">
    {{ creating ? 'Cadastrando...' : 'Cadastrar corrida' }}
  </button>

  <p v-if="successMessage" class="success">
    {{ successMessage }}
  </p>

  <p v-if="formError" class="error">
    {{ formError }}
  </p>
</form>
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
    error: null,

    newRace: {
      name: '',
      location: '',
      distance: null
    },

    creating: false,
    successMessage: '',
    formError: ''
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
        const response = await axios.get('http://127.0.0.1:8000/races')
        console.log('Dados recebidos da API:', response.data)
        this.races = response.data
      } catch (error) {
        console.error('Erro ao buscar corridas:', error)
        this.error =
          'Não foi possível carregar as corridas. Verifique se a API está funcionando.'
      } finally {
        this.loading = false
      }
    },

    async createRace() {
      this.creating = true
      this.successMessage = ''
      this.formError = ''

      try {
        const response = await axios.post(
          'http://127.0.0.1:8000/races',
          this.newRace
        )

        this.successMessage = 'Corrida cadastrada com sucesso!'
        this.newRace = {
          name: '',
          location: '',
          distance: null
        }

        await this.fetchRaces()
        console.log('Corrida cadastrada:', response.data)
      } catch (error) {
        console.error('Erro ao cadastrar corrida:', error)
        this.formError =
          'Não foi possível cadastrar a corrida. Verifique os dados e tente novamente.'
      } finally {
        this.creating = false
      }
    }
  }
}
</script>

<style scoped>
h2 {
  color: #2c3e50;
  margin-bottom: 20px;
}

.races-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
  gap: 16px;
}

.race-card {
  background-color: white;
  border: 1px solid #dfe6e9;
  border-radius: 10px;
  padding: 18px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.08);
}

.race-card h3 {
  color: #42b983;
  margin-top: 0;
}

.race-card p {
  color: #555;
  margin-bottom: 8px;
}

.race-form {
  background-color: white;
  padding: 20px;
  border-radius: 10px;
  margin-bottom: 25px;
  border: 1px solid #dfe6e9;
}

.race-form h3 {
  margin-top: 0;
}

.form-group {
  display: flex;
  flex-direction: column;
  margin-bottom: 15px;
  text-align: left;
}

.form-group label {
  margin-bottom: 6px;
  font-weight: bold;
}

.form-group input {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 14px;
}

.race-form button {
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 5px;
  padding: 12px 18px;
  cursor: pointer;
  font-size: 15px;
}

.race-form button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.success {
  color: #218838;
  margin-top: 12px;
}

.error {
  color: #c0392b;
  margin-top: 12px;
}
</style>