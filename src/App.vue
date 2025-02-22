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
    <h2>Dejdag</h2>
    <p>Dejen skal laves ~48 timer før du vil bage.</p>
    <ol class="recipe-step-list">
      <li>Hæld <strong>vand</strong> i en skål (f.eks. din røremaskines)</li>
      <li>Opløs <strong>gæren</strong> i vandet</li>
      <li>Hæld <strong>melet</strong> i</li>
      <li><strong>Ælt</strong> i ca. 2 minutter</li>
      <li>Tilsæt <strong>saltet</strong> når dejen har samlet sig</li>
      <li><strong>Ælt</strong> i ca. 10 minutter</li>
      <li>Hæld dejen ud på bordet og lad den <strong>slappe af</strong> i ca. 20 minutter — det giver alt det magiske gluten ro til at gøre sin ting, hvilket gør dejen helt glat og smidig.</li>
      <li>Form dejen til <strong>en stor kugle</strong></li>
      <li>Læg dejen i en lufttæt <strong>hævekasse</strong></li>
      <li>Sæt hævekassen <strong>på køl</strong> i 24-48 timer — jo længere, jo bedre</li>
    </ol>
    <h2>Bagedag</h2>
    <p>Start 3-5 timer før du vil bage.</p>
    <ol class="recipe-step-list">
      <li>Tag hævekassen med dejen <strong>ud af køleskabet</strong></li>
      <li><strong>Del dejen</strong> i {{ pizzas }} lige store stykker</li>
      <li>Form dejen til <strong>flotte kugler</strong> og læg dem i en lufttæt hævekasse</li>
      <li>Vent 2-4 timer</li>
      <li>Læg dit <strong>bagestål i ovnen</strong></li>
      <li><strong>Tænd ovnen</strong> på grill og højeste temperatur (jeg plejer at bage på 300℃, men 4-500℃ er bedst)</li>
      <li>Forbered din topping</li>
      <li><strong>Lav og bag</strong> dine pizzaer en ad gangen. De skal bage i 1-8 minutter alt efter hvor varm din ovn er og hvor meget topping du har fyldt på. Det kan være en god ide at lade ovnen varme bagestålet i et par minutter mellem pizzaerne, så det ikke når at blive for koldt undervejs.</li>
    </ol>
    <p>Baseret på <a href="https://jonsmadklub.dk/italiensk-pizzadej/">Jons italienske pizzadej</a>.</p>
  </main>
</template>

<style scoped>
main {
  padding: 5vh 5vw;
  max-width: 40rem;
}

.input-wrapper {
  display: inline-flex;
  align-items: center;
  gap: .5rem;
}

.recipe-step-list {
  display: flex;
  flex-direction: column;
  gap: 2rem;
  margin-block: 2rem;
  counter-reset: recipe-steps;

  > li {
    position: relative;
    display: block;
    margin-left: 0;
    padding-left: 3rem;
    counter-increment: recipe-steps;

    &::before {
      content: counter(recipe-steps);
      display: inline-block;
      width: 2rem;
      height: 2rem;
      line-height: 2rem;
      margin-left: -3rem;
      margin-right: 1rem;
      text-align: center;
      background: var(--divider-color);
      border-radius: 100%;
      left: -2rem;
      z-index: -1;
    }

    &:not(:last-child) {
      &::after {
        content: "";
        position: absolute;
        display: block;
        width: 25%;
        left: 3rem;
        bottom: -1rem;
        border: 2px solid var(--divider-color);
        border-radius: 2px;
      }
    }
  }
}
</style>
