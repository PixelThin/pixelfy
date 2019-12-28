<template>
  <v-autocomplete label="Country" :items="countries" :value="value" @input="handleInput"></v-autocomplete>
</template>
<script>
import axios from 'axios'
export default {
  data() {
    return {
      countries: ['U.S.A.', 'Other'],
    }
  },
  props: ['value'],
  methods: {
    handleInput(e) {
      this.$emit('input', e)
    }
  },
  async mounted() {
    const countriesResponse = await axios.get('https://restcountries.eu/rest/v2/all')
    this.countries = countriesResponse.data.
      map((c) => c.name)
      .sort()
  },
}
</script>
