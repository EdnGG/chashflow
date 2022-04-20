<template>
  <div>
    <svg
      @touchstart="tap"
      @touchmove="tap"
      @touchend="untap"
      viewBox="0 0 300 200"
    >
      <line 
        stroke="#c4c4c4"
        stroke-width="2"
        x1="0"
        :y1="zero"
        x2="300"
        :y2="zero"
      />
      <polyline 
        fill="none"
        stroke="#0689B0"
        stroke-width="2"
        :points="points"
      />
      <line 
        v-show="showPointer"
        stroke="#04b500"
        stroke-width="2"
        :x1="pointer"
        y1="0"
        :x2="pointer"
        y2="200"
      />  
    </svg>
    <p>Last 30 days</p>
    <div>{{ amounts }}</div>
  </div>
</template>

<script setup>
import { defineProps, toRefs, defineEmits, computed, ref, watch } from "vue";

const props = defineProps({
  amounts: {
    type: Array,
    default: () => [],
  },
});

const { amounts } = toRefs(props);

const amountToPixels = (amount) => {
  // Esto funciona porque amounts es global
  const min = Math.min(...amounts.value);
  const max = Math.max(...amounts.value);

  const amountAbs = amount + Math.abs(min);
  const minmax = Math.abs(max) + Math.abs(min);

  /* 
    calcula el numero de pixeles y lo convertimos a porcentaje
    tomar el numero de pixeles y restarle el tamano en pixeles de la grafica
  */
  return 200 - ((amountAbs * 100) / minmax) * 2;
};

const zero = computed(() => {
  return amountToPixels(0);
});

const points = computed(() => {
  const total = amounts.value.length;
  return amounts.value.reduce((points, amount, index) => {
    const x = (300 / total) * (index + 1);
    const y = amountToPixels(amount);
    // console.log(y);
    return `${points} ${x},${y}`;
  }, `0, ${amountToPixels(amounts.value.length ? amounts.value[0] : 0)}`);
});

const showPointer = ref(false);
const pointer = ref(0);

const emits = defineEmits(["select"]);
//
watch(pointer, (value) => {
  const index = Math.ceil(value / (300 / amounts.value.length));
  if (index < 0 || index > amounts.value.length) {
    return;
  }
  emits("select", amounts.value[index - 1]);
});

const tap = ({ target, touches }) => {
  /*                
    Objetivo
    Obtener el ancho del componente (grafica)
    Obtener la coordenada en donde inicia
  */

  /*
    Funcion de JS que nos permite obtener la posicion del elemento
  */
  showPointer.value = true;
  const elementWidth = target.getBoundingClientRect().width;
  const elementX = target.getBoundingClientRect().x;
  const touchX = touches[0].clientX;
  pointer.value = ((touchX - elementX) * 300) / elementWidth;

  // emits("select", pointer.value);
  /*
    elementWidth = Tamano real de elemento
    touchX = distancia desde la orilla izquierda de la pantalla hasta el toque
    elementX = distancia desde la orilla izquierda de la pantalla hasta el elemento
  */
};

const untap = () => {
  showPointer.value = false;
};
</script>


<style scoped>
svg {
  width: 100%;
}
p {
  text-align: center;
}
</style>