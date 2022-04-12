<template>
  <Layout>
    <template #header>
       <Header></Header>
    </template>
    <template #resume>
      <Resume
        :total-label=" 'Total Savings' "
        :label="label"
        :amount="amount"
        :totalAmount="1000000"
      >
        <template #graphic>
          graphic
        </template>  
        <template #action>
          action
        </template>   

      </Resume>
    </template>
    <template #movements>
      <Movements
        :movements="movements"
      
      />
    </template>
  </Layout>
 
</template>


<script>
import Layout from "./Layout.vue";
import Header from "./Header.vue";
import Resume from "./Resume/Index.vue";
import Movements from "./Movements/Index.vue";

export default {
  components: {
    Layout,
    Header,
    Resume,
    Movements,
  },
  data() {
    return {
      amount: null,
      label: null,
      movements: [
        {
          id: 1,
          title: "Movement 1",
          description: "lorem ipsum",
          amount: 100,
        },
        {
          id: 2,
          title: "Movement 2",
          description: "lorem ipsum",
          amount: 200,
        },
        {
          id: 3,
          title: "Movement 3",
          description: "lorem ipsum ",
          amount: 300,
        },
        {
          id: 4,
          title: "Movement 4",
          description: "lorem sdfsdfds ",
          amount: 400,
        },
        {
          id: 5,
          title: "Movement 5",
          description: "loremd hdjfkhjksdf ",
          amount: 500,
        },
      ],
    };
  },
};
</script>