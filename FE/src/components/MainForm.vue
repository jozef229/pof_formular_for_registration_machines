<template>
  <v-container fluid>
    <v-row justify="center" align="center" class="row">
      <v-col cols="8" sm="12" md="8" xs="12" class="text-left">
        <h1 class="mt-2">Vloženie záznamu</h1>
        <p class="mb-8">Prosím vyplňte všetky potrebné údaje</p>

        <v-form ref="form" class="mx-2" v-model="valid" lazy-validation>
          <v-row>
            <v-col cols="12">
              <v-combobox
                v-model="selectCustomer"
                :items="itemsCustomer"
                label="Zákazník"
                :rules="requiredRules"
                solo
                required
              ></v-combobox>
            </v-col>

            <v-col cols="12">
              <div class="text-h5 grey--text transition-swing">
                Zústatek kreditú:
              </div>
            </v-col>

            <v-col cols="12">
              <v-combobox
                v-model="selectContact"
                :items="itemsContact"
                :rules="requiredRules"
                label="Kontakt"
              ></v-combobox>
            </v-col>

            <v-col cols="6">
              <v-text-field
                v-model="telephone"
                :rules="telephoneRules"
                label="Telefon bez predcisli"
                required
              ></v-text-field>
            </v-col>

            <v-col cols="6">
              <v-text-field v-model="email" label="E-mail"></v-text-field>
            </v-col>

            <v-col cols="12 mb-4">
              <v-combobox
                v-model="selectCreator"
                :items="itemsCreator"
                :rules="requiredRules"
                label="Vytvoril"
                dense
              ></v-combobox>
            </v-col>

            <v-col cols="12">
              <v-sheet class="pa-12" color="grey lighten-4" rounded="true">
                <v-row>
                  <v-col cols="12">
                    <v-combobox
                      v-model="selectTool"
                      :items="itemsTool"
                      :rules="requiredRules"
                      label="Stroj"
                      dense
                    ></v-combobox>
                  </v-col>

                  <v-menu
                    ref="menu"
                    v-model="menuFrom"
                    :close-on-content-click="false"
                    :return-value.sync="dateFrom"
                    transition="scale-transition"
                    offset-y
                    min-width="auto"
                  >
                    <template v-slot:activator="{ on, attrs }">
                      <v-text-field
                        :rules="requiredRules"
                        v-model="dateFrom"
                        label="Zapújčení od"
                        prepend-icon="mdi-calendar"
                        readonly
                        v-bind="attrs"
                        v-on="on"
                      ></v-text-field>
                    </template>
                    <v-date-picker v-model="dateFrom" no-title scrollable>
                      <v-spacer></v-spacer>
                      <v-btn text color="primary" @click="menuFrom = false">
                        Cancel
                      </v-btn>
                      <v-btn
                        text
                        color="primary"
                        @click="$refs.menuFrom.save(dateFrom)"
                      >
                        OK
                      </v-btn>
                    </v-date-picker>
                  </v-menu>

                  <v-menu
                    ref="menu"
                    v-model="menuTo"
                    :close-on-content-click="true"
                    :return-value.sync="dateTo"
                    transition="scale-transition"
                    offset-y
                    min-width="auto"
                  >
                    <template v-slot:activator="{ on, attrs }">
                      <v-text-field
                        :rules="requiredRules"
                        v-model="dateTo"
                        label="Zapujčaní do"
                        prepend-icon="mdi-calendar"
                        readonly
                        v-bind="attrs"
                        v-on="on"
                      ></v-text-field>
                    </template>
                    <v-date-picker v-model="dateTo" no-title scrollable>
                      <v-spacer></v-spacer>
                      <v-btn text color="primary" @click="menuTo = false">
                        Cancel
                      </v-btn>
                      <v-btn
                        text
                        color="primary"
                        @click="$refs.menuTo.save(dateTo)"
                      >
                        OK
                      </v-btn>
                    </v-date-picker>
                  </v-menu>
                </v-row>
              </v-sheet>
              <v-btn class="mt-6" elevation="4">Pridať vec</v-btn>
            </v-col>
          </v-row>

          <v-col cols="12 mb-4">
            <v-combobox
              v-model="selectVEK"
              :items="itemsVEK"
              :rules="requiredRules"
              label="VEK Pobočka"
            ></v-combobox>
          </v-col>

          <v-col cols="12 mb-4">
            <v-combobox
              v-model="selectVEKNumber"
              :items="itemsVEKNumber"
              :rules="requiredRules"
              label="Číslo VEK objednávky"
            ></v-combobox>
          </v-col>

          <v-col cols="12">
            <v-row>
              <v-checkbox
                v-model="checkboxExtension"
                label="Jedna se o prodlouzeni"
              ></v-checkbox>

              <v-btn class="darken-1 black--text mt-3 ml-4">
                Odkaz na XXXX
              </v-btn>
            </v-row>
          </v-col>
          <v-row>
            <v-col cols="3">
              <v-btn class="darken-1 black--text mt-5" @click="submitForm">
                Uložiť záznam
              </v-btn>
            </v-col>
            <v-col cols="3">
              <v-btn class="darken-1 black--text mt-5"> E-mail zákazník </v-btn>
            </v-col>
            <v-col cols="3">
              <v-btn class="darken-1 black--text mt-5"> E-mail VEK </v-btn>
            </v-col>
          </v-row>
        </v-form>
      </v-col>

      <v-col
        cols="12"
        sm="12"
        md="12"
        xs="12"
        class="mt-4 text-center darken-2"
      >
        <v-divider></v-divider>
        <h1 class="mb-6 mt-5">Aktuálne záznamy zákazníka</h1>
        <v-card>
          <v-card-title>
            <v-text-field
              v-model="search"
              append-icon="mdi-magnify"
              label="Search"
              single-line
              hide-details
            ></v-text-field>
          </v-card-title>
          <v-data-table
            :headers="headers"
            :items="desserts"
            :search="search"
          ></v-data-table>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "RegisterForm",
  data: () => ({
    valid: true,
    dateFrom: "",
    menuFrom: false,

    dateTo: "",
    menuTo: false,
    selectContact: "",
    itemsContact: ["Jozef JJJ", "Ladislav ASD", "Martin AEF", "Ondrej EFWEF"],
    selectTool: "",
    itemsTool: ["DD 234", "TE 23452345 2345", "TE 6-234523", "ASD 6-A22s"],
    selectCustomer: "",
    itemsCustomer: ["XXX", "CCC", "DDD stav", "FFF"],
    selectCreator: "",
    itemsCreator: ["Vargová", "Mareček", "Lapka", "Špundová"],
    telephone: "",
    selectVEK: "",
    itemsVEK: ["Dečín", "Praha", "Brno", "Ostrava"],

    selectVEKNumber: "",
    itemsVEKNumber: ["123", "234", "345", "456"],
    requiredRules: [(v) => !!v || "Toto pole nesmie zostať prázdne"],
    telephoneRules: [
      (v) => !!v || "Phone number is required",
      (v) => /^(([0-9]\d{9}))$/.test(v) || "Phone number must be valid",
    ],
    email: "",
    emailRules: [
      (v) => !!v || "E-mail is required",
      (v) =>
        /^(([^<>()[\]\\.,;:\s@']+(\.[^<>()\\[\]\\.,;:\s@']+)*)|('.+'))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/.test(
          v
        ) || "E-mail must be valid",
    ],
    checkboxExtension: false,

    search: "",
    headers: [
      {
        text: "Timestamp",
        align: "start",
        value: "timestamp",
      },
      { text: "ID VEK", value: "id_VEK" },
      { text: "Stroje", value: "tools" },
      { text: "Pujčeno od", value: "time_from" },
      { text: "Pujčeno do", value: "time_to" },
      { text: "Číslo špičky", value: "id" },
      { text: "Dnú", value: "date" },
      { text: "Kredity", value: "credit" },
      { text: "Zákazník", value: "customer" },
      { text: "BP", value: "bp" },
      { text: "Kontakt zákazníka", value: "contact" },
      { text: "Tel. zákazníka", value: "phone" },
      { text: "E-mail", value: "email" },
      { text: "VEK SSS", value: "VEK_shop" },
      { text: "VEK email", value: "VEK_email" },
      { text: "Vytvoril", value: "created" },
      { text: "Prodloučení", value: "extension" },
    ],
    desserts: [
      {
        timestamp: "10.1.2022",
        id_VEK: "123",
        tools: "TE 30",
        time_from: "10.1.2022",
        time_to: "12.1.2022",
        id: "1235",
        date: "9.1.2022",
        credit: "500",
        customer: "ASD",
        bp: "12345",
        contact: "Jozef ASFDS",
        phone: "92349293234",
        email: "jozef@gmail.com",
        VEK_shop: "Decin",
        VEK_email: "VEK@VEK.sk",
        Created: "Vargová",
        extension: "12",
      },
    ],
  }),
  methods: {
    submitForm() {
      this.$refs.form.validate();
    },
  },
};
</script>
<style scoped>
.fluid {
  margin: 0;
  padding: 0;
}
</style>