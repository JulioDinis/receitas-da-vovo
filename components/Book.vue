<template>
  <v-card class="mx-auto">
    <v-container fluid>
      <v-row dense>
        <v-col v-for="livro in livros" :key="livro.id" :cols="3">
          <v-card>
            <v-img
              :src="`https://api.polemicnews.com${livro.photo.download}`"
              class="white--text align-end"
              gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)"
              height="400px"
            >
              <v-card-title v-text="livro.title"></v-card-title>
            </v-img>

            <v-card-actions>
              <v-spacer></v-spacer>

              <v-btn
                v-if="livro.documents[0].downloadable"
                icon
                @click="baixar(livro.documents[0].download)"
              >
                <v-icon>mdi-download-box-outline</v-icon>
              </v-btn>

              <v-btn icon>
                <v-icon>mdi-telegram</v-icon>
              </v-btn>

              <v-btn icon>
                <v-icon>mdi-share-variant</v-icon>
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </v-card>
</template>
<script>
export default {
  props: {
    livros: {
      type: Array,
      default() {},
    },
  },
  data() {
    return {}
  },
  methods: {
    baixar(link) {
      window.location = `https://api.polemicnews.com${link}`
    },
  },
}
</script>
