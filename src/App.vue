<template>
  <main>
    <Titulo />
    <h2 v-if="carregando">Carregando ...</h2>
    <h2 v-else>Jogador da vez: {{ currentPlayer }}</h2>
    <Game
      :jogadorAtual="currentPlayer"
      :jogou="jogou"
      :temVencedor="!!vencedor"
      :opcoes="opcoes"
      :carregando="carregando"
    />
    <h1 v-if="vencedor">O jogador {{ vencedor }} venceu a rodada!</h1>
    <h1 v-if="empate">O jogo empatou</h1>
    <button v-if="vencedor || empate" @click="reiniciarJogo">
      Reiniciar o jogo
    </button>
  </main>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Game, { type Iopcao } from "./components/Game.vue";
import Titulo from "./components/Titulo.vue";
import axios from "axios";

const valoresIniciais = {
  currentPlayer: "X",
  vencedor: "",
  empate: false,
  opcoes: [
    { opcao: 1, player: "" },
    { opcao: 2, player: "" },
    { opcao: 3, player: "" },
    { opcao: 4, player: "" },
    { opcao: 5, player: "" },
    { opcao: 6, player: "" },
    { opcao: 7, player: "" },
    { opcao: 8, player: "" },
    { opcao: 9, player: "" },
  ],
};

export default defineComponent({
  name: "App",
  components: {
    Titulo,
    Game,
  },
  data() {
    return {
      currentPlayer: valoresIniciais.currentPlayer,
      vencedor: valoresIniciais.vencedor,
      empate: valoresIniciais.empate,
      opcoes: [...valoresIniciais.opcoes],
      carregando: false,
    };
  },
  methods: {
    async jogou(opcao: Iopcao) {
      opcao.player = this.currentPlayer;

      this.carregando = true;

      const resposta = await axios.post(
        "http://localhost:2222/verifica-Jogo",
        this.opcoes
      );

      this.vencedor = resposta.data.vencedor;
      this.empate = resposta.data.empate;

      this.carregando = false;

      // const opcoesX = this.opcoes
      //   .filter((opcao) => opcao.player === "X")
      //   .map((opcao) => opcao.opcao);

      // const opcoesO = this.opcoes
      //   .filter((opcao) => opcao.player === "O")
      //   .map((opcao) => opcao.opcao);

      // for (const jogo of jogos) {
      //   if (jogo.every((item) => opcoesX.includes(item))) {
      //     this.vencedor = "X";
      //     return;
      //   }
      //   if (jogo.every((item) => opcoesO.includes(item))) {
      //     this.vencedor = "O";
      //     return;
      //   }
      // }

      // if (opcoesX.length + opcoesO.length === 9) {
      //   this.empate = true;
      // }

      this.currentPlayer = this.currentPlayer === "O" ? "X" : "O";
    },
    reiniciarJogo() {
      console.log(valoresIniciais.opcoes);
      this.currentPlayer = valoresIniciais.currentPlayer;
      this.vencedor = valoresIniciais.vencedor;
      this.empate = valoresIniciais.empate;
      this.opcoes = valoresIniciais.opcoes.map((opt) => ({
        ...opt,
        player: "",
      }));
    },
  },
});
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Lora", serif;
}

h1 {
  color: red;
}

h2 {
  padding-left: 50px;
}
main {
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  background-image: url("./assets/catedrall.png");
  background-size: cover;
  gap: 20px;
}

button {
  padding: 10px 20px;
  background-color: darkslategrey;
  border-radius: 5px;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  font-weight: bold;
  color: aliceblue;
  border: none;
  font-size: 12px;
  letter-spacing: 150%;
  text-transform: uppercase;
  box-shadow: 0 10px 10px -10px black;
  transition: 200ms;
  cursor: pointer;
}
button:hover {
  transform: scale(0.95);
}
</style>
