<template>
  <v-container class="mt-6">
    <v-card
      elevation="2"
    >
    <FormDetail />
    <v-divider></v-divider>
    <FormProduct />
    </v-card>    
  </v-container>
</template>

<script>
  import FormDetail from "./FormDetail.vue";
  import FormProduct from "./FormProduct.vue";
  export default {
    name: 'HelloWorld',
    components: {
      FormDetail,
      FormProduct
    },
  }
</script>
