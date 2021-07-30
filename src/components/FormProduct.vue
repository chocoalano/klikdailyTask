<template>
  <div>
    <v-row>
      <v-col cols="12" md="4" sm="4">
        <v-container>
          <p>Product</p>
        </v-container>
      </v-col>
      <v-col cols="12" md="8" sm="8">
        <v-container>
          <div
            class="mt-6"
            v-for="input in exampleMultipleData"
            :key="input.id"
          >
            <v-row dense>
              <v-col cols="12" md="1">
                <v-btn
                  class="mx-2"
                  fab
                  dark
                  small
                  color="error"
                  @click="deleteInput"
                >
                  <v-icon> mdi-delete-empty </v-icon>
                </v-btn>
              </v-col>
              <v-col cols="12" md="7">
                <v-select
                  :id="input.id"
                  v-model="input.product"
                  :items="itemsselectproduct"
                  item-text="product_name"
                  item-value="units"
                  label="Product"
                  single-line
                  outlined
                  dense
                  required
                ></v-select>
              </v-col>
              <v-col cols="12" md="4">
                <v-select
                  :id="input.id"
                  v-model="input.unit"
                  :items="(input.product === null)?[]:input.product"
                  item-text="name"
                  item-value="price"
                  label="Unit"
                  single-line
                  outlined
                  dense
                  required
                  v-on:change="input.price = input.unit"
                ></v-select>
              </v-col>
            </v-row>
            <v-row dense>
              <v-col cols="12" md="2">
                <v-text-field
                  :id="input.id"
                  v-model="input.qty"
                  label="Quantity"
                  single-line
                  outlined
                  dense
                  required
                  @keyup="function(){
                    input.total = input.price*input.qty;
                    input.totalfix = input.total;
                    grntotal=exampleMultipleData.reduce((acc,item)=>acc + item.total,0);
                    disabled=(grntotal>0 && input.total !== '')?false:true
                  }"
                />
              </v-col>
              <v-col cols="12" md="4">
                <v-text-field
                  :id="input.id"
                  v-model="input.price"
                  label="Price"
                  single-line
                  outlined
                  dense
                  disabled
                  required
                />
              </v-col>
              <v-col cols="12" md="6">
                <v-text-field
                  :id="input.id"
                  v-model="input.total"
                  label="Total Price"
                  disabled
                  single-line
                  outlined
                  dense
                  required
                />
              </v-col>
            </v-row>
            <v-row dense>
              <v-col cols="12" md="6" offset-md="6">
                <v-divider></v-divider>
              </v-col>
            </v-row>
            <v-row dense>
              <v-col cols="12" md="6" offset-md="6">
                <v-row class="d-flex justify-space-between mt-2 ml-2 mr-2">
                  <p class="font-weight-bold">Total Net Price</p>
                  <p class="font-weight-bold">Rp. {{ (input.totalfix) }}</p>
                </v-row>
              </v-col>
            </v-row>
            <v-row>
              <v-btn
                color="yellow darken-4"
                class="ma-2 white--text"
                @click="addInput"
              >
                New Item
                <v-icon> mdi-plus </v-icon>
              </v-btn>
            </v-row>
          </div>
        </v-container>
      </v-col>
    </v-row>
    <v-row class="d-flex justify-end mb-5">
      <v-col cols="12" md="8">
        <v-row dense>
          <v-col cols="12" md="6" offset-md="6">
            <v-divider></v-divider>
          </v-col>
        </v-row>
        <v-row dense>
          <v-col cols="12" md="6" offset-md="6">
            <v-row class="d-flex justify-space-between mt-2 ml-2 mr-2">
              <p class="font-weight-bold">Total</p>
              <p class="font-weight-bold">Rp. {{ (grntotal/1).toFixed(2).replace('.', ',').toString().replace(/\B(?=(\d{3})+(?!\d))/g, '.') }}</p>
            </v-row>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
    <v-divider></v-divider>
    <v-card-actions class="d-flex justify-end" dense>
      <v-btn @click="clear"> Cancel </v-btn>
      <v-btn class="mr-4 success" @click="submit" :disabled="disabled"> Confirm </v-btn>
    </v-card-actions>
  </div>
</template>
<script>
export default {
  name: "Example",
  data: () => {
    return {
      disabled: true,
      counter: 0,
      exampleMultipleData: [
        {
          id: 'p0',
          product: null,
          unit: null,
          qty: 0,
          price: 0,
          total: 0,
          totalfix:0,
        },
      ],
      grntotal:0,
      itemsselectproduct: [
        {
          product_name: "Morning Dew Milk",
          units: [{ name: "Pcs", price: 1000 }],
        },
        {
          product_name: "Le Minerale 600ml",
          units: [
            { name: "Karton", price: 100000 },
            { name: "Pak", price: 10000 },
          ],
        },
        {
          product_name: "Greenfields Full Cream Milk 1L",
          units: [
            { name: "Karton", price: 100000 },
            { name: "Pak", price: 10000 },
            { name: "Pcs", price: 1000 },
          ],
        },
      ],
    };
  },
  methods: {
    submit() {
      alert("good")
    },
    clear() {
      const r = {
        product: null,
        unit: null,
        qty: 0,
        price: 0,
        total: 0,
      };
      this.exampleMultipleData = r;
    },
    addInput() {
      this.exampleMultipleData.push({
        id: `p${++this.counter}`,
        product: null,
        unit: [],
        qty: 0,
        price: 0,
        total: 0,
      });
      this.counter=++this.counter
    },
    deleteInput() {
      const array=this.exampleMultipleData.indexOf(this.counter);
      this.exampleMultipleData.splice(array,1);
      this.grntotal=this.exampleMultipleData.reduce((acc,item)=>acc + item.total,0);
    },
  },
};
</script>

<style scoped>
</style>