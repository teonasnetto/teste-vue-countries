<template>
  <div>
    <main class="max-w-7xl mx-auto mt-10 px-4">
      <nav class="flex flex-col md:flex-row space space-y-4 md:space-y-0 justify-between items-center">
        <div class="w-full md:w-auto relative px-4 py-1 flex items-center bg-gray-700 rounded">
            <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8z" clip-rule="evenodd"></path></svg>
          <input type="text" name="" id="" class="px-2 py-1 bg-gray-700 border-none border-gray-800 outline-none rounded" v-model="search">
        </div>
        <h2 class="font-semibold text-2xl">Total: {{countries ? countries.length : ""}}</h2>
        <div class="w-full md:w-40 relative h-full py-1 flex items-center bg-gray-700 rounded">
            <select class="w-full md:w-40 h-full px-4 py-1 bg-gray-700 border-none border-gray-800 outline-none rounded" name="" id="" v-model="filterByRegion" @change="handleChangeFilter">
              <option class="text-sm font-semibold" v-for="(filter, index) in filters" :key="index">
                {{ filter }}
              </option>
            </select>
        </div>
      </nav>
      <section class="mt-10" v-if="!error">
        <div v-if="loading === true" class="w-full flex justify-center items-center pt-16">
          <svg class="animate-spin -ml-1 mr-2 h-16 w-16 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
            <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
            <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
          </svg>
        </div>
        <div v-else>
          <div class="flex flex-wrap" v-if="countries && countries.length > 0">
            <article v-for="(country, index) in countries" :key="index" class="flex-shrink-0 w-1/2 md:w-1/6 p-2">
              <router-link :to="String(country.ccn3)" class="block bg-gray-700 rounded">
                <img :src="country.flags.png" alt="" class="object-cover w-full h-28">
                <div class="px-2 py-2.5">
                  <p class="text-lg font-semibold mb-1 truncate">{{country.name.common}}</p>
                  <p><strong>População:</strong> {{country.population}}</p>
                  <p><strong>Região:</strong> {{country.region}}</p>
                  <p><strong>Capital:</strong> {{[...country.capital][0]}}</p>
                </div>
              </router-link>
            </article>
          </div>
          <div v-else>
            <h2>Não Encontrado. ;(</h2>
          </div>
        </div>
      </section>
      <section v-else class="flex justify-center items-center">
        <p class="mt-8 text-2xl font-semibold">
          {{ messageError }}
        </p>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: 'HomeView',

  data() {
    return {
      error: false,
      messageError: "",
      loading: false,
      countries: [],
      allCountries: [],
      search: "",
      filterByRegion: "All",
      filters: [
        'All',
        'Africa',
        'America',
        'Asia',
        'Europe',
        'Oceania',
      ]
    }
  },
  methods: {
    async getCountries() {
      const response = await fetch("https://restcountries.com/v3.1/all");
      const json = await response.json();
      
      this.countries = [...json]
      this.allCountries = this.countries;
    },
    searchCountries(value) {
      
      const searchContruies = this.countries.filter((item) => {
        return item.name.common.toLowerCase().includes(value.toLowerCase());
      })

      if (searchContruies.length > 0) {
        this.countries = [...searchContruies];
      }

      if (value === "") {
        this.filterByRegion = "All"
        this.getCountries();
      }

    },
    handleChangeFilter($event) {
      const value = $event.target.value;

      const filterCountries = this.allCountries.filter((item) => {
        if (item.region.includes(value)) {
          return item
        }
      })

      if (filterCountries.length > 0) {
        this.countries = [...filterCountries];
      }

      if (value == "All") {
        this.countries = this.allCountries
      }
    },
  },
  computed: {
  
  },
  async mounted() {
   try {
    this.loading = true
    await this.getCountries();
   } catch (erro) {
    this.messageError = "Ocorreu um Erro!"
    this.error = true;
   } finally {
    this.loading = false
   }
  },
  watch: {
    search(newValue) {
    this.searchCountries(newValue)
   }
  },
}
</script>
