<script setup lang="ts">
import { NeuralNetwork } from 'brain.js'
import { onMounted, ref } from 'vue'
const net = new NeuralNetwork()

const inputWord = ref<string | null>(null)
const hasTheTestingStarted = ref(false)
const isInputWordComplex = ref(false)

function hashCode(word: string) {
  let hash = 0
  if (word.length === 0) return hash
  for (let i = 0; i < word.length; i++) {
    const char = word.charCodeAt(i)
    hash = (hash << 5) - hash + char
    hash &= hash // Convert to 32bit integer
  }
  return hash
}

function analyzeWord() {
  if (!inputWord.value) return
  hasTheTestingStarted.value = true
  const result = net.run({ word: hashCode(inputWord.value) }) as { isComplex: number }
  isInputWordComplex.value = result.isComplex > 0.5
}

onMounted(() => {
  net.train([
    { input: { word: hashCode('Circunvalación') }, output: { isComplex: 1 } },
    { input: { word: hashCode('Inexorable') }, output: { isComplex: 1 } },
    { input: { word: hashCode('Obfuscación') }, output: { isComplex: 1 } },
    { input: { word: hashCode('Perspicaz') }, output: { isComplex: 1 } },
    { input: { word: hashCode('Axiomático') }, output: { isComplex: 1 } },
    { input: { word: hashCode('Cacofonía') }, output: { isComplex: 1 } },
    { input: { word: hashCode('Epistemología') }, output: { isComplex: 1 } },
    { input: { word: hashCode('Inefable') }, output: { isComplex: 1 } },
    { input: { word: hashCode('Quimérico') }, output: { isComplex: 1 } },
    { input: { word: hashCode('Vicisitudes') }, output: { isComplex: 1 } },
    { input: { word: hashCode('Efervescente') }, output: { isComplex: 1 } },
    { input: { word: hashCode('Disparate') }, output: { isComplex: 1 } },
    { input: { word: hashCode('Polimatía') }, output: { isComplex: 1 } },
    { input: { word: hashCode('Proclividad') }, output: { isComplex: 1 } },
    { input: { word: hashCode('Antítesis') }, output: { isComplex: 1 } },
    { input: { word: hashCode('Inextricable') }, output: { isComplex: 1 } },
    { input: { word: hashCode('Anacronismo') }, output: { isComplex: 1 } },
    { input: { word: hashCode('Falacia') }, output: { isComplex: 1 } },
    { input: { word: hashCode('Mellifluo') }, output: { isComplex: 1 } },
    { input: { word: hashCode('Cognitivamente') }, output: { isComplex: 1 } },
    { input: { word: hashCode('Casa') }, output: { isComplex: 0 } },
    { input: { word: hashCode('Perro') }, output: { isComplex: 0 } },
    { input: { word: hashCode('Gato') }, output: { isComplex: 0 } },
    { input: { word: hashCode('Sol') }, output: { isComplex: 0 } },
    { input: { word: hashCode('Azul') }, output: { isComplex: 0 } },
    { input: { word: hashCode('Mar') }, output: { isComplex: 0 } },
    { input: { word: hashCode('Comida') }, output: { isComplex: 0 } },
    { input: { word: hashCode('Agua') }, output: { isComplex: 0 } },
    { input: { word: hashCode('Jugar') }, output: { isComplex: 0 } },
    { input: { word: hashCode('Correr') }, output: { isComplex: 0 } },
    { input: { word: hashCode('Feliz') }, output: { isComplex: 0 } },
    { input: { word: hashCode('Rápido') }, output: { isComplex: 0 } },
    { input: { word: hashCode('Dulce') }, output: { isComplex: 0 } },
    { input: { word: hashCode('Flor') }, output: { isComplex: 0 } },
    { input: { word: hashCode('Niño') }, output: { isComplex: 0 } },
    { input: { word: hashCode('Luna') }, output: { isComplex: 0 } },
    { input: { word: hashCode('Árbol') }, output: { isComplex: 0 } },
    { input: { word: hashCode('Amarillo') }, output: { isComplex: 0 } },
    { input: { word: hashCode('Hola') }, output: { isComplex: 0 } },
    { input: { word: hashCode('Grande') }, output: { isComplex: 0 } }
  ])
})
</script>

<template>
  <p>Esta red neuronal analiza una palabra y menciona si es compleja o simple</p>
  <label for="word">Ingresa una palabra en español:</label>
  <input v-model="inputWord" type="text" />
  <button @click="analyzeWord">Analizar</button>

  <h2
    :class="{ green: !isInputWordComplex, orange: isInputWordComplex }"
    v-if="hasTheTestingStarted"
  >
    {{ isInputWordComplex ? 'Es compleja!' : 'Es simple!' }}
  </h2>
</template>

<style scoped>
p {
  font-weight: bold;
  font-size: 2rem;
}
.green {
  color: green;
}

.orange {
  color: orange;
}
</style>
