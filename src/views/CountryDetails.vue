<template>
  <v-container>
    <v-row>
      <v-col>
        <v-btn class="btn" @click="goBack">
          <v-icon>mdi-arrow-left</v-icon> Back</v-btn>
      </v-col>
    </v-row>
    
    <v-row>
      <v-col md="6" sm="12" xs="12">
        <v-img :src="country.flag"></v-img>
      </v-col>

      <v-col md="6" sm="12" xs="12" class="pa-2">
        <div class="mt-8">
          <h2>{{ country.name }}</h2>
          <v-row class="mt-3">
            <v-col lg="12" md="12" sm="12" xs="12">
              <div
                :class="`d-flex justify-space-between flex-wrap `"
                style="height: 100%"
              >
                <div>
                  <div>
                    <b>Native Name: </b
                    >{{ country.nativeName ? country.nativeName : "NA" }}
                  </div>
                  <div>
                    <b>Population: </b
                    >{{ country.population | numberFormatter }}
                  </div>
                  <div><b>Region: </b>{{ country.region }}</div>
                  <div><b>Sub Region: </b>{{ country.subregion }}</div>
                  <div><b>Capital: </b>{{ country.capital }}</div>
                  <div><b>CallingCodes: </b>{{ country.callingCodes[0] }}</div>
                </div>
                <div>
                  <div><b>Alpha2Code: </b>{{ country.alpha2Code }}</div>
                  <div><b>Alpha3Code: </b>{{ country.alpha3Code }}</div>
                  <div><b>AltSpellings: </b>{{ country.altSpellings[1] }}</div>

                  <div>
                    <b>Top Level Domain: </b
                    ><span
                      class="mr-2"
                      v-for="domain in country.topLevelDomain"
                      :key="domain"
                      >{{ domain }}</span
                    >
                  </div>
                  <div>
                    <b>Currencies: </b
                    ><span
                      class="mr-2"
                      v-for="currency in country.currencies"
                      :key="currency.code"
                      >{{ country.name }}</span
                    >
                  </div>
                  <div>
                    <b>Languages: </b
                    ><span
                      class="mr-2"
                      v-for="language in country.languages"
                      :key="language.name"
                      >{{ language.name }}
                    </span>
                  </div>
                </div>
              </div>
            </v-col>
          </v-row>


        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      country: {},
      code: null,
    };
  },
  mounted() {
    this.init();
  },
  methods: {
    init() {
      if (this.$route.params.code) {
        let data = atob(this.$route.params.code);
        this.code = data;
        this.getCountryDetails();
      }
    },
    getCountryDetails() {
      this.$http
        .get(`alpha/${this.code}`)
        .then((response) => {
          if (response.status == 200) {
            const data = response.data;
            this.country = data;
          }
        })
        .catch((error) => {
          console.log(error);
        });
    },
    goBack() {
      this.$router.push({
        name: "Home",
      });
    },
    getBorder(border) {
      this.code = border;
      this.getCountryDetails();
    },
  },
};
</script>

<style>
.theme--light.v-app-bar.v-toolbar.v-sheet{
  height:70px !important;
}
.v-application--wrap{
      min-height: auto !important;
}
</style>