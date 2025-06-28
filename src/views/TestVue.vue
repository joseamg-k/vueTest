<script setup lang="ts">
import { ref } from 'vue'

const count = ref(0)
const mensaje = ref('Incrementar')
const mensajeinput = ref('Hola mensaje desde input...')
const valorInput = ref('')

const lista = [
  { id: 11, text: 'item en objecto lista 1' },
  { id: 22, text: 'item en objecto lista 2' },
  { id: 33, text: 'item en objecto lista 3' },
  { id: 44, text: 'item en objecto lista 4' },
]

function increment() {
  count.value++
}

function resetcount() {
  count.value = 0
}

function eventoChangeSelect(event: Event) {
  const target = event.target as HTMLSelectElement
  valorInput.value = target.value
}
</script>

<template>
  <div class="pruebas">
    <h1>Contador: {{ count }}</h1>

    <a class="btn green" @click="increment">{{ mensaje }}</a>
    <span style="padding-right: 50px"></span>
    <a class="btn green" @click="resetcount">Reiniciar</a>

    <br />
    <p style="padding-top: 15px">
      prabando usando un input: <span class="green"> {{ mensajeinput }} </span> <br />
    </p>
    Escriba el nuevo msj: <input class="form-control" v-model="mensajeinput" />
    <br />

    <select class="form-select" @change="eventoChangeSelect">
      <option value="-99" selected>Selecione un item</option>
      <option v-for="item in lista" v-bind:value="item.id">{{ item.text }}</option>
    </select>
    <p v-show="valorInput">Valor del item selecionado: {{ valorInput }}</p>
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .Pruebas {
    padding-top: 40vh;
    min-height: 100vh;
    display: flexbox;
    align-items: center;
  }
}
</style>
