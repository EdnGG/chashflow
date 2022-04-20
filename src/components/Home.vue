<template>
  <Layout>
    <template #header>
      <Header></Header>
    </template>
    <template #resume>
      <Resume
        :total-label="'Total Savings'"
        :label="label"
        :total-amount="totalAmount"
        :amount="amount"
      >
        <template #graphic>
          <Graphic 
            :amounts="amounts"
            @select="select"   
          />
        </template>
        <template #action>
          <Action @create="create" />
        </template>
      </Resume>
    </template>
    <template #movements>
      <Movements :movements="movements" @remove="remove" />
    </template>
  </Layout>
</template>

<script>
import Layout from "./Layout.vue";
import Header from "./Header.vue";
import Resume from "./Resume/Index.vue";
import Movements from "./Movements/Index.vue";
import Action from "./Action.vue";
import Graphic from "./Resume/Graphic.vue";

export default {
  components: {
    Layout,
    Header,
    Resume,
    Movements,
    Action,
    Graphic,
  },
  data() {
    return {
      amount: null,
      label: null,
      movements: [],
    };
  },
  mounted() {
    const movements = JSON.parse(localStorage.getItem("movements"));

    if (Array.isArray(movements)) {
      this.movements = movements.map((m) => {
        return { ...m, time: new Date(m.time) };
      });
    }
  },
  methods: {
    select(el) {
      this.amount = el;
    },
    save() {
      localStorage.setItem("movements", JSON.stringify(this.movements));
    },
    create(movement) {
      this.movements.push(movement);
      this.save();
    },
    remove(id) {
      const index = this.movements.findIndex((m) => m.id === id);
      this.movements.splice(index, 1);
      this.save();
    },
  },
  computed: {
    totalAmount() {
      return this.movements.reduce((total, m) => {
        return total + m.amount;
      }, 0);
    },
    amounts() {
      const lastDays = this.movements
        .filter((m) => {
          const today = new Date();
          // const oldDate = today.setDate(today.getDate() - 30);
          const oldDate = today.getDate() - 30;
          console.log("today ", today);
          console.log("oldDate: ", oldDate);
          console.log("m.time ", m.time);
          // Aqui deveria ser >= pero no funciona
          // devolvemos todos los elementos ue sean menores a los ultimos 30 dias (<)
          return m.time > oldDate;
        })
        .map((m) => m.amount);

      return lastDays.map((m, i) => {
        // Con la linea de abajo siempre va a dibujar el movimiento mas reciente
        const lastMovements = lastDays.slice(0, i + 1);

        return lastMovements.reduce((suma, movement) => {
          return suma + movement;
        }, 0);
      });
    },
  },
};
</script>
