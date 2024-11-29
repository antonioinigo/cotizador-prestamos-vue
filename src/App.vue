<script setup>
import {ref, computed, watch} from 'vue';
import Header from './components/Header.vue'
import Button from './components/Button.vue'
import {calcularTotalPagar} from './helpers'

const cantidad = ref(10000);
  const meses = ref(6);
  const total = ref(0);
  const MIN = 0;
  const MAX = 20000;
  const STEP = 100;

  const formatearDinero = (valor) => {
  const formatter = new Intl.NumberFormat('es-ES', {
    style: 'currency',
    currency: 'EUR'
  });
  return formatter.format(valor);
};

  watch([cantidad, meses], () => {
    total.value = calcularTotalPagar(cantidad.value, meses.value);
  });

  const pagoMensual = computed(() => {
      return total.value / meses.value
  });


const handleChangeDecremento = () => {
    const valor = cantidad.value - STEP;
    if( valor < MIN ) {
      alert('Cantidad no válida');
      return;
    }
    cantidad.value = valor;
  }

  const handleChangeIncremento = () => {
    const valor = cantidad.value + STEP;
    if( valor > MAX ) {
      alert('Cantidad no válida');
      return;
    }
    cantidad.value = valor;
  }


</script>

<template>
  <div class="my-20 max-w-lg mx-auto bg-white shadow p-10">
    <Header />

    <div class="flex justify-between mt-10">
        <Button
          :operador="'-'"
          @fn="handleChangeDecremento"
        />
        <Button 
          :operador="'+'"
          @fn="handleChangeIncremento"
        />
      </div>

    <div class="my-5">
      <input 
      type="range"
      class="w-full bg-gray-200 accent-lime-500 hover:bg-lime-600"
      :min="MIN"
      :max="MAX"
      :step="STEP"
      v-model.number="cantidad"
      />

      <p class="text-center my-10 text-5xl font-extrabold text-indigo-600"> 
        {{formatearDinero(cantidad) }}
      </p>

      <h2 class="text-2xl font-extrabold text-center text-gray-500">
        Elige un <span class="text-indigo-600">Plazo</span> a Pagar?
      </h2>
      <select 
            class="w-full p-2 bg-white border border-gray-300 rounded-lg text-center text-xl font-bold text-gray-500 mt-5"
            :value="meses"
            v-model.number="meses"
          >
              <option value="6">6 Meses</option>
              <option value="12">12 Meses</option>
              <option value="24">24 Meses</option>

      </select>
    </div>

    <div class="my-5 space-y-3 p-5 bg-gray-50">
      <h2 class="text-2xl font-extrabold text-center text-gray-500">
        Resumen de tu <span class="text-indigo-600">Préstamo</span>
      </h2>
      <p class="text-xl font-bold text-gray-500">
        Cantidad a Solicitar: <span class="text-indigo-600">{{formatearDinero(cantidad)}}</span>
      </p>
      <p class="text-xl font-bold text-gray-500">
        Plazo a Pagar: <span class="text-indigo-600">{{meses}} Meses</span>
      </p>
      <p class="text-xl font-bold text-gray-500">
        Total a Pagar: <span class="text-indigo-600">{{formatearDinero( total)}}</span>
      </p>
      <p class="text-xl font-bold text-gray-500">
        Pago Mensual: <span class="text-indigo-600">{{formatearDinero(pagoMensual)}}</span>
      </p>



    </div>
  </div>
</template>


