<template>
  <v-container>
    <v-row>
      <v-col>
        <v-text-field
          label="Search"
          v-model="search"
          solo
          prepend-inner-icon="mdi-magnify"
          clearable
          @click:clear="clearSearch"
        >
        </v-text-field
      ></v-col>

      <v-spacer class="d-none d-md-flex d-lg-flex"></v-spacer>

      <v-col>
        <v-select
          v-model="region"
          @change="regionChange"
          label="Filter by region"
          :items="regions"
          item-text="name"
          item-value="code"
          solo
          clearable
        >
        </v-select>
      </v-col>

    </v-row>

    <v-row>
      <v-col v-for="country in countries" :key="country.alpha3Code" md="3">
        <country-card v-bind="country"></country-card>
      </v-col>

       <paginate-links
      for="country"
      :limit="2"
      :show-step-links="true"></paginate-links>

    </v-row>

  </v-container>
</template>

<script>
import CountryCard from "../components/CountryCard.vue";
// import axios from "axios";

export default {
  name: "Home",
  components: {
    "country-card": CountryCard,
  },
  data() {
    return {
      countries: [],
      paginate: ["countries"],
      search: null,
      region: null,
      regions: ["Africa", "Americas", "Asia", "Europe", "Oceania"],
    };
  },
  created() {
      this.loadPressRelease();
    },

  mounted() {
    this.getCountries();
  },
  watch: {
    search: {
      immediate: true,
      handler(val) {
        if (val) {
          this.region = null;
          this.getCountries();
        }
      },
    },
  },
  methods: {
    regionChange() {
      this.search = null;
      this.getCountries();
    },
    clearSearch() {
      setTimeout(() => {
        this.search = null;
        this.getCountries();
      }, 100);
    },
    getCountries() {
      let url = "all";
      if (this.search) {
        url = `name/${this.search}`;
      } else if (this.region) {
        url = `region/${this.region.toLowerCase()}`;
      }
      setTimeout(() => {
        this.callAPI(url);
      });
    },
    callAPI(url) {
      this.$http
        .get(url)
        .then((response) => {
          if (response.status == 200) {
            this.countries = response.data;
          }
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>


<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
ul.flex-container {
  padding: 0;
  margin: 0;
  list-style-type: none;
  display: -webkit-box;
  display: -moz-box;
  display: -ms-flexbox;
  display: -webkit-flex;
  display: flex;
  -webkit-flex-flow: row wrap;
  flex-direction: row wrap;
  flex-wrap: wrap;
  justify-content: space-around;
}
li img {
  display: initial;
  height: 100px;
}
.flex-item {
  background: tomato;
  width: calc(100% / 3.5);
  padding: 5px;
  height: auto;
  margin-top: 10px;
  color: white;
  font-weight: bold;
  text-align: center;
}
.downloads {
  margin-top: 10px;
}
ul.paginate-links.items li {
  display: inline-block;
  margin: 5px;
}
ul.paginate-links.items a {
  cursor: pointer;
}
ul.paginate-links.items li.active a {
  font-weight: bold;
}
ul.paginate-links.items li.next:before {
  content: " | ";
  margin-right: 13px;
  color: #ddd;
}
ul.paginate-links.items li.disabled a {
  color: #ccc;
  cursor: no-drop;
}
</style>