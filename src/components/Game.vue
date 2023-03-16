<template>
  <div class="game">
    <button
      v-for="opcao in opcoes"
      @click="() => aoClicar(opcao)"
      :disabled="temVencedor"
    >
      {{ opcao.player }}
    </button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export interface Iopcao {
  opcao: number;
  player: string;
}

export default defineComponent({
  name: "Game",
  props: {
    jogadorAtual: {
      type: String,
      required: true,
    },
    jogou: {
      type: Function,
      required: true,
    },
    temVencedor: Boolean,
    opcoes: {
      type: Array as () => Iopcao[],
      required: true,
    },
  },
  methods: {
    aoClicar(opcao: Iopcao) {
      if (opcao.player) {
        return;
      }

      this.jogou(opcao);
    },
  },
});
</script>

<style>
.game {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}

.game button {
  width: 100px;
  height: 100px;
  margin: 5px;
  cursor: pointer;
  font-size: 50px;
  background-color: rgba(30, 30, 30, 0.5);
  border-radius: 10px;
  box-shadow: -5px 4px 2px rgb(131, 131, 131);
  color: white;
}
</style>
