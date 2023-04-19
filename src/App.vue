<script setup>
  import { ref, watch, computed } from 'vue';
  import Header from "./components/Header.vue";
  import Button from './components/Button.vue';
  import { calcularTotalPagar } from './utils'

  const cantidad = ref(10000);
  const meses = ref(6);
  const total = ref(0);

  const MIN = 0;
  const MAX = 20000;
  const STEP = 100;

  const formatearDinero = (valor) => {
    const formatter = new Intl.NumberFormat('en-US', {
      style: 'currency',
      currency: 'USD',
    });

    return formatter.format(valor);
  };

  watch([cantidad, meses], () => {
    total.value = calcularTotalPagar(cantidad.value, meses.value);
  });

  function handleChangeDecremento() {
    const valor = cantidad.value - STEP;

    if( valor < MIN ) {
      alert('Cantidad no valida');
      return;
    }

    cantidad.value = valor;
  }

  function handleChangeIncremento() {
    const valor = cantidad.value + STEP
    
    if( valor > MAX ) {
      alert('Cantidad no valida')
      return;
    }

    cantidad.value = valor
  }

  const pagoMensual = computed(() =>  {
    return total.value / meses.value;
  });

</script>

<template>
  <div class="my-20 max-w-lg mx-auto bg-white shadow p-10">
    <Header />

    <div class="flex justify-between mt-10">
      <Button 
        :operator="'-'"
        :fn="handleChangeDecremento"
      />

      <Button 
        :operator="'+'"
        :fn="handleChangeIncremento"
      />
    
    </div>

    <div class="my-5">
      <input 
        type="range" 
        id="cantidad" 
        class="bg-gray-200 w-full accent-lime-500 hover:accent-lime-600"
        :min="0"
        :max="MAX"
        :step="STEP"
        v-model.number="cantidad"
      />
    </div>

    <p class="text-center my-10 text-indigo-600 font-extrabold text-5xl">
      {{formatearDinero(cantidad)}}
    </p>

    <h2 class="text-2xl font-extrabold text-gray-500 text-center">
      Elige un <span class="text-indigo-600">plazo</span> a pagar
    </h2>

    <select
      class="w-full p-2 bg-white border border-gray-300 rounded-lg text-center text-xl font-bold text-gray-600 mt-5 outline-0"
      :value="meses"
      v-model.number="meses"
    >
      <option value="6">6 Meses</option>
      <option value="12">12 Meses</option>
      <option value="24">24 Meses</option>
    </select>

    <div v-if="total > 0" class="my-5 space-y-5 bg-gray-50 p-5">
      <h2 class="text-2xl font-extrabold text-gray-500 text-center">
        Resumen <span class="text-indigo-600">de pagos</span>
      </h2>

      <p class="text-center text-xl text-gray-600 font-bold">{{meses}} Meses</p>
      <p class="text-center text-xl text-gray-600 font-bold">
        Total a pagar: {{formatearDinero(total)}}
      </p>
      <p class="text-center text-xl text-gray-600 font-bold">
        Mensuales: 
      </p>
    
    </div>

  </div>
</template>

