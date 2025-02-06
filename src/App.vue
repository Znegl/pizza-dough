<script setup>
import {computed, ref} from "vue";

const pizzas = ref(4)
const flour = ref(600)
const ratio = ref(Math.round(415 / 600 * 100))

const water = computed(() => flour.value * ratio.value / 100)

const dryYeast = ref(false)
const yeast = computed(() => flour.value / 600 * 2 * (dryYeast.value ? 11.8 / 50 : 1))
const salt = computed(() => Math.round(flour.value / 600 * 16))

const totalWeight = computed(() => flour.value + water.value + yeast.value + salt.value)
const doughPerPizza = computed(() => Math.floor(totalWeight.value / pizzas.value))
</script>

<template>
  <main>
    <h1>Pizza&shy;dej</h1>
    <table>
      <tbody>
      <tr>
        <th>Antal pizzaer</th>
        <td>
          <label class="input-wrapper">
            <input type="number" v-model="pizzas" autofocus>
            stk.
          </label>
        </td>
      </tr>
      <tr>
        <th>Mel</th>
        <td>
          <label class="input-wrapper">
            <input type="number" v-model="flour">
            gram
          </label>
        </td>
      </tr>
      <tr>
        <th>Vandprocent</th>
        <td>
          <label class="input-wrapper">
            <input type="range" v-model="ratio" min="0" max="100" step="1">
            {{ ratio }}%
          </label>
        </td>
      </tr>
      <tr>
        <th>Vand</th>
        <td>
          <span class="input-wrapper">
            <output>{{ water }}</output>
            gram
          </span>
        </td>
      </tr>
      <tr>
        <th>Tørgær</th>
        <td>
          <label class="input-wrapper">
            <input type="checkbox" v-model="dryYeast">
            Ja
          </label>
        </td>
      </tr>
      <tr>
        <th>Gær</th>
        <td>
          <span class="input-wrapper">
            <output>{{
              yeast.toLocaleString('da', {
                style: 'unit',
                unit: 'gram',
                unitDisplay: 'long',
                maximumFractionDigits: 1
              })
            }}
          </output>
          </span>
        </td>
      </tr>
      <tr>
        <th>Salt</th>
        <td>
          <span class="input-wrapper">
            <output>{{ salt }}</output>
            gram
          </span>
        </td>
      </tr>
      <tr>
        <th>Dej/pizza</th>
        <td>
          <span class="input-wrapper">
            <output>{{ doughPerPizza }}</output>
            gram
          </span>
        </td>
      </tr>
      </tbody>
    </table>
    <ol>
      <li>Hæld <strong>vand</strong> og <strong>gær</strong> i en skål (f.eks. din røremaskines)</li>
      <li>Opløs <strong>gæren</strong></li>
      <li>Hæld <strong>melet</strong> i</li>
      <li><strong>Ælt</strong> i ca. 2 minutter</li>
      <li>Tilsæt <strong>saltet</strong></li>
      <li><strong>Ælt</strong> i ca. 10 minutter</li>
    </ol>
    <p>Baseret på <a href="https://jonsmadklub.dk/italiensk-pizzadej/">Jons italienske pizzadej</a>.</p>
  </main>
</template>

<style scoped>
main {
  padding: 1rem;
}

.input-wrapper {
  display: inline-flex;
  align-items: center;
  gap: .5rem;
}
</style>
