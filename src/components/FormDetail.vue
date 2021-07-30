<template>
  <v-row>
    <v-col cols="12" md="4" sm="4">
      <v-container>
        <p>Detail</p>
      </v-container>
    </v-col>
    <v-col cols="12" md="8" sm="8">
      <v-container>
        <v-row dense>
          <v-col cols="12" md="8">
            <v-select
              v-model="selectname"
              :items="itemsselectname"
              :error-messages="selectErrorsName"
              item-text="employee_name"
              item-value="employee_name"
              label="Name"
              single-line
              outlined
              dense
              required
              @change="$v.selectname.$touch()"
              @blur="$v.selectname.$touch()"
            ></v-select>
          </v-col>
        </v-row>
        <v-row dense>
          <v-col cols="12" md="6">
            <v-select
              v-model="selectdistribution"
              :items="selectname ? itemdistribution : []"
              :error-messages="selectErrorsDistribution"
              label="Distribution Center"
              single-line
              outlined
              dense
              required
              @change="$v.selectdistribution.$touch()"
              @blur="$v.selectdistribution.$touch()"
            ></v-select>
          </v-col>
        </v-row>
        <v-row dense>
          <v-col cols="12" md="6">
            <v-select
              v-model="selectpaytype"
              :items="itempaytype"
              :error-messages="selectErrorsPayType"
              label="Payment Type"
              single-line
              outlined
              dense
              required
              @change="$v.selectpaytype.$touch()"
              @blur="$v.selectpaytype.$touch()"
            ></v-select>
          </v-col>
          <v-col cols="12" md="6">
            <v-menu
              ref="menuexpdate"
              v-model="menuexpdate"
              :close-on-content-click="false"
              :return-value.sync="expdate"
              transition="scale-transition"
              offset-y
              min-width="auto"
            >
              <template v-slot:activator="{ on, attrs }">
                <v-text-field
                  v-model="expdate"
                  :error-messages="selectErrorsExpDate"
                  label="Expired Date"
                  single-line
                  outlined
                  dense
                  required
                  v-bind="attrs"
                  v-on="on"
                  @change="$v.expdate.$touch()"
                  @blur="$v.expdate.$touch()"
                ></v-text-field>
              </template>
              <v-date-picker v-model="expdate" no-title scrollable>
                <v-spacer></v-spacer>
                <v-btn text color="primary" @click="menuexpdate = false">
                  Cancel
                </v-btn>
                <v-btn
                  text
                  color="primary"
                  @click="$refs.menuexpdate.save(expdate)"
                >
                  OK
                </v-btn>
              </v-date-picker>
            </v-menu>
          </v-col>
        </v-row>
        <v-row dense>
          <v-col cols="12" md="8">
            <v-textarea
              v-model="notes"
              :error-messages="ErrorsNotes"
              label="Notes"
              single-line
              outlined
              dense
              required
              @change="$v.notes.$touch()"
              @blur="$v.notes.$touch()"
            >
            </v-textarea>
          </v-col>
        </v-row>
      </v-container>
    </v-col>
  </v-row>
</template>
<script>
import { validationMixin } from "vuelidate";
import { required } from "vuelidate/lib/validators";

export default {
  mixins: [validationMixin],

  validations: {
    selectname: { required },
    selectdistribution: { required },
    selectpaytype: { required },
    expdate: { required },
    notes: { required },
  },

  data: () => ({
    selectname: null,
    selectdistribution: null,
    selectpaytype: null,
    expdate: "",
    notes: "",

    menuexpdate: false,
    itemsselectname: [],
    itemdistribution: ["DC Tangerang", "DC Cikarang"],
    itempaytype: [
      "Cash H+1",
      "Cash H+3",
      "Cash H+7",
      "Transfer H+1",
      "Transfer H+3",
      "Transfer H+7",
    ],
    itemUnits: [],
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
  }),

  mounted() {
    this.axios
      .get("http://dummy.restapiexample.com/api/v1/employees")
      .then((response) => {
        this.itemsselectname = response.data.data;
      });
  },

  computed: {
    selectErrorsName() {
      const errors = [];
      if (!this.$v.selectname.$dirty) return errors;
      !this.$v.selectname.required && errors.push("Name is required");
      return errors;
    },
    selectErrorsDistribution() {
      const errors = [];
      if (!this.$v.selectdistribution.$dirty) return errors;
      !this.$v.selectdistribution.required &&
        errors.push("Distribution is required");
      return errors;
    },
    selectErrorsPayType() {
      const errors = [];
      if (!this.$v.selectpaytype.$dirty) return errors;
      !this.$v.selectpaytype.required &&
        errors.push("Payment Type is required");
      return errors;
    },
    selectErrorsExpDate() {
      const errors = [];
      if (!this.$v.expdate.$dirty) return errors;
      !this.$v.expdate.required && errors.push("Expired Date is required");
      return errors;
    },
    ErrorsNotes() {
      const errors = [];
      if (!this.$v.notes.$dirty) return errors;
      !this.$v.notes.required && errors.push("Notes is required");
      return errors;
    },
  },

  methods: {
    selectProduct() {
      this.itemUnits = this.product;
    }
  },
};
</script>