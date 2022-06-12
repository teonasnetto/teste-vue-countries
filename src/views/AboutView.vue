<template>
  <div class="about">
   <main class="max-w-7xl mx-auto mt-10 px-4">
       <nav class="flex flex-col md:flex-row space space-y-4 md:space-y-0 justify-between items-center">
         <router-link to="/" class="w-full md:w-auto relative px-4 py-1 flex items-center bg-gray-700 rounded">
            <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm.707-10.293a1 1 0 00-1.414-1.414l-3 3a1 1 0 000 1.414l3 3a1 1 0 001.414-1.414L9.414 11H13a1 1 0 100-2H9.414l1.293-1.293z" clip-rule="evenodd"></path></svg>
          <div id="" class="px-2 py-0.5 bg-gray-700 border-none border-gray-800 outline-none rounded font-semibold text-lg">Back</div>
        </router-link>
      </nav>
      <section class="mt-10">
        <div class="w-full flex justify-between items-stretch">
          <div class="flex-1">
            <img :src="country.flags.png" alt="" class="w-full object-cover">
          </div>
          <div class="flex-1 pl-24">
            <h2 class="font-semibold text-2xl">{{country.name.common}}</h2>
            <div class="flex flex-wrap mt-10 space-y-4">
              <div class="w-1/2">
                <p><strong>Native Name</strong>: {{Object.values(country.name.nativeName)[0].official}}</p>
              </div>
              <div class="w-1/2">
                <p><strong>Population</strong>: {{country.population}}</p>
              </div>
              <div class="w-1/2">
                <p><strong>Region</strong>: {{country.region}}</p>
              </div>
              <div class="w-1/2">
                <p><strong>Language</strong>: {{Object.values(country.languages)[0]}}</p>
              </div>
              <div class="w-full">
                <div class="space-x-2">
                  <strong>Border Countries</strong>:
                 <span class="inline-block bg-gray-800 hover:bg-gray-900 transition-all ease-in-out px-2 py-1.5 mb-2 rounded cursor-pointer" v-for="(borderCountry, index) in country.borders" :key="index"> {{borderCountry}}</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>
   </main>
  </div>
</template>

<script>

export default {
  data() {
    return {
      error: false,
      messageError: "",
      loading: false,
      country: {},
    }
  },
  methods: {
    async getCountry(code) {
      const response = await fetch(`https://restcountries.com/v3.1/alpha/${code}`);
      const json = await response.json();

      this.country = { ...json[0] }
    }
  },
  async mounted() {
   try {
    this.loading = true
    await this.getCountry(this.$route.params.id);
   } catch (erro) {
    this.messageError = "Ocorreu um Erro!"
    this.error = true;
   } finally {
    this.loading = false
   }
  },
}
</script>