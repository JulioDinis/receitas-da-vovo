<template>
  <v-row>
    <v-col class="text-center">
      <blockquote class="blockquote">
        <v-text-field v-model="query" label="Busca"></v-text-field>
        <v-btn color="success" @click="busca(query)"> buscar </v-btn>
        <v-spacer />
        <div v-if="buscaRealizada">
          <h3>
            Foram encontradas
            {{ resultados.results.total_results }} receitas, bom apetite!
          </h3>
        </div>
        <div v-else>
          <h3>
            {{
              naoEncontrado
                ? 'Sinto muito, não estou conseguindo encontrar, busque novamente'
                : `Digite a receita desejada`
            }}
          </h3>
        </div>

        <v-spacer />
        <v-spacer />
        <book v-if="buscaRealizada" :livros="livros" />
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
      retornouAlgo: false,
      naoEncontrado: false,
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
        .get(`/s/author?query= ${query}&page=${page}&max_results=1`)
        .then((response) => {
          console.log(response)
          this.resultados = response.data
          this.livros = this.resultados.results.results
          if (typeof this.livros !== 'undefined') {
            this.buscaRealizada = true
            console.log(this.livros)
            console.log('resultados')
            console.log(this.resultados)
          } else {
            alert('jasdkajsdk')
          }
        })
        .catch((response) => {
          this.naoEncontrado = true
        })
    },
  },
}
</script>
