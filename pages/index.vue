<template>
  <div>
    <v-card-title><h3>あなたの好きな本を教えてください</h3></v-card-title>
    <v-row justify="center">
      <v-col v-for="(book,i) in books" :key="i" cols="12" sm="8" md="4">
        <v-card shaped :to="'book?id='+i">
          <v-card-text>{{book.passage}}</v-card-text>
          <v-card-actions>
              <v-spacer />
              <v-card-title>
                {{ book.title }}
              </v-card-title>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
    <v-row>
      <v-spacer />
      <v-btn fab color="accent" elevation="11" to="/bookInput">
        <v-icon color="primary">
          mdi-plus
        </v-icon>
      </v-btn>
    </v-row>
  </div>
</template>

<script lang="ts">
import { Vue, Component } from "nuxt-property-decorator";
import { getDatabase, ref, onValue } from "firebase/database";
@Component
export default class Index extends Vue {
  books = "";

  // インスタンス初期化後に実行
  created() {
    this.getBooks();
  }

  // 本の情報を全て取得する
  getBooks(this: any){
    const db = getDatabase(this.$firebase);
    const data = ref(db, "books");
    onValue(data, (snapshot) => {
      const books = snapshot.val();
      this.books = books;
    })
  }
}
</script>


