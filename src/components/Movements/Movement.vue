<template>
  <div class="movement">
    <div class=content>
      <h4>{{ title }}</h4>
      <p>{{ description }}</p>
    </div>
    <div class="action">
      <img src="@/assets/trash-icon.svg" alt="Delete" @click="remove()" />
      <p :class="{ 
        'red': isNegative, 
        'green': !isNegative 
      }">{{ amountCurrency }}</p>
    </div>
   
  </div>
</template>

<script setup>
import { computed, defineProps, toRefs, defineEmits } from "vue";

const currencyFormatter = new Intl.NumberFormat("en-US", {
  style: "currency",
  currency: "USD",
});

const props = defineProps({
  title: {
    type: String,
  },
  id: {
    type: Number,
  },
  description: {
    type: String,
  },
  amount: {
    type: Number,
  },
});

// Esto se tiene que hacer para que la varible sea reactiva
const { id, title, description, amount } = toRefs(props);

const amountCurrency = computed(() => currencyFormatter.format(amount.value));

const isNegative = computed(() => amount.value < 0);

const emit = defineEmits(["remove"]);

const remove = () => {
  console.log(id.value);
  emit("remove", id.value);
};
</script>

<style scoped>
.movement {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 16px;
  background-color: #e6f9ff;
  border-radius: 8px;
  box-sizing: border-box;
}
.movement .content {
  width: 100%;
}
.movement .action {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  flex-direction: column;
}
h4,
p {
  margin: 0;
  padding: 0;
}
h4 {
  margin-bottom: 8px;
}
.movement .action img {
  margin-bottom: 16px;
}
.red {
  color: red;
}
.green {
  color: green;
}
</style>