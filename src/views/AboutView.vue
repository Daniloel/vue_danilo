<template>
  <div class="character-form">
    <h2>Cadastro de Personagem</h2>
    <form @submit.prevent="cadastrarPersonagem">
      <label for="nome">Nome:</label>
      <input id="nome" type="text" v-model="nome" />
      <label for="classe">Classe:</label>
      <input id="classe" type="text" v-model="classe" />
      <label for="forca">Força:</label>
      <input id="forca" type="number" v-model="forca" />
      <label for="inteligencia">Inteligência:</label>
      <input id="inteligencia" type="number" v-model="inteligencia" />
      <button type="submit">Cadastrar</button>
      <button type="button" @click="limparCampos">Limpar</button>
    </form>

    <h2>Pesquisar Personagens</h2>
    <input type="text" v-model="termoPesquisa" placeholder="Digite o termo de pesquisa" />
    <button @click="pesquisar">Pesquisar</button>

    <h2>Personagens Cadastrados</h2>
    <table>
      <thead>
        <tr>
          <th>Id</th>
          <th>Nome</th>
          <th>Classe</th>
          <th>Força</th>
          <th>Inteligência</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="personagem in personagens" :key="personagem.id">
          <td>{{ personagem.id }}</td>
          <td>{{ personagem.nome }}</td>
          <td>{{ personagem.classe }}</td>
          <td>{{ personagem.forca }}</td>
          <td>{{ personagem.inteligencia }}</td>
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
      classe: '',
      forca: null,
      inteligencia: null,
      personagens: [],
      termoPesquisa: ''
    };
  },
  methods: {
    async cadastrarPersonagem() {
      try {
        const response = await axios.post('/api/personagens', {
          nome: this.nome,
          classe: this.classe,
          forca: this.forca,
          inteligencia: this.inteligencia
        });

        const personagem = response.data;
        this.personagens.push(personagem);

        this.limparCampos();
      } catch (error) {
        console.error(error);
      }
    },
    async getPersonagens() {
      try {
        const response = await axios.get('/api/personagens');
        this.personagens = response.data;
      } catch (error) {
        console.error(error);
      }
    },
    pesquisar() {
      if (this.termoPesquisa) {
        const termo = this.termoPesquisa.toLowerCase();
        this.personagens = this.personagens.filter(personagem => {
          return (
            personagem.nome.toLowerCase().includes(termo) ||
            personagem.classe.toLowerCase().includes(termo)
          );
        });
      } else {
        this.getPersonagens();
      }
    },
    limparCampos() {
      this.nome = '';
      this.classe = '';
      this.forca = null;
      this.inteligencia = null;
    }
  },
  mounted() {
    this.getPersonagens();
  }
};
</script>

<style>
.character-form {
  margin-top: 20px;
}

table {
  margin-top: 20px;
  width: 100%;
}

th,
td {
  padding: 8px;
  text-align: left;
}
</style>
