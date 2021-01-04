<template>
  <v-row>
    <v-col class="text-center">
      <blockquote class="blockquote">
        <v-text-field v-model="query" label="Busca"></v-text-field>
        <v-btn color="success" @click="busca(query)"> buscar </v-btn>
        <book :livros="livros" />
        <v-pagination
          v-if="buscaRealizada"
          v-model="page"
          :length="resultados.pagination.total_pages"
          :total-visible="7"
        >
        </v-pagination>
      </blockquote>
    </v-col>
  </v-row>
</template>
<script>
import Book from '@/components/Book'
export default {
  layout: 'receitas',
  components: {
    Book,
  },
  data() {
    return {
      query: '',
      livros: [],
      resultados: Object,
      page: 1,
      buscaRealizada: false,
    }
  },
  watch: {
    page(novapagina) {
      this.busca(this.query, novapagina - 1)
    },
  },
  methods: {
    busca(query, page) {
      this.$axios
        .get(`/s/title?query= ${query}&page=${page}&max_results=16`)
        .then((response) => {
          this.resultados = response.data
          this.buscaRealizada = true
          this.livros = this.resultados.results.results
          console.log(this.livros)
          console.log('resultados')
          console.log(this.resultados)
        })
    },
  },
}
</script>
