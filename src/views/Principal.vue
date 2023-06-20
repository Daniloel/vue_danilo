<template>
  <div>
    <h2>Cadastro</h2>
    <form @submit.prevent="submitForm">
      <label for="name">Nome:</label>
      <input type="text" id="name" v-model="nome" required>

      <label for="age">Idade:</label>
      <input type="number" id="age" v-model="idade" required>

      <label for="city">Cidade:</label>
      <input type="text" id="city" v-model="cidade" required>

      <button type="submit">Cadastrar</button>
    </form>

    <h2>Registros</h2>
    <table>
      <thead>
        <tr>
          <th>Nome</th>
          <th>Idade</th>
          <th>Cidade</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(registro, index) in registros" :key="index">
          <td>{{ registro.nome }}</td>
          <td>{{ registro.idade }}</td>
          <td>{{ registro.cidade }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      nome: '',
      idade: '',
      cidade: '',
      registros: []
    };
  },
  methods: {
    async submitForm() {
      try {
        // Enviar a requisição POST para cadastrar o registro
        await axios.post('/api/registros', {
          nome: this.nome,
          idade: this.idade,
          cidade: this.cidade
        });

        // Limpar os campos de entrada
        this.nome = '';
        this.idade = '';
        this.cidade = '';

        // Atualizar a lista de registros chamando o método GET
        this.getRegistros();
      } catch (error) {
        console.error(error);
      }
    },
    async getRegistros() {
      try {
        // Enviar a requisição GET para obter os registros cadastrados
        const response = await axios.get('/api/registros');
        this.registros = response.data;
      } catch (error) {
        console.error(error);
      }
    }
  },
  mounted() {
    // Ao carregar o componente, chamar o método GET para obter os registros cadastrados
    this.getRegistros();
  }
};
</script>
