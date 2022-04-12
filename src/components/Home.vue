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
      <Movements/>
    </template>
  </Layout>
 
</template>


<script>
import Layout from "./Layout.vue";
import Header from "./Header.vue";
import Resume from "./Resume/Index.vue";
import Movements from "./Movements.vue";

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
    };
  },
};
</script>