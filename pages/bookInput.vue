<template>
  <div>
    <Form pagetitle="本の情報を入力" input />
    <br>
    <v-card>
      <v-list>
        <v-list-item
          v-for="(book, i) in books"
          :key="i"
          :to="'book?id='+i"
        >
        {{ book.title }}：{{ book.author }}
        </v-list-item>
      </v-list>
    </v-card>
  </div>
</template>

<script lang="ts">
import { Vue, Component } from "nuxt-property-decorator";
import { getDatabase, ref, onValue, set, push } from "firebase/database";
@Component
export default class BookShelf extends Vue {
  books = "";
  // 本の情報を全て取得する
  getBooks(this: any){
    const db = getDatabase(this.$firebase);
    const data = ref(db, "books");
    onValue(data, (snapshot) => {
      const books = snapshot.val();
      this.books = books;
    })
  }

  // インスタンス初期化後に実行
  created() {
    this.getBooks();
  }
}
</script>


