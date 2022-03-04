<template>
  <div>
    <v-data-table
      :headers="tableHeader"
      :items="books"
      :items-per-page="10"
      class="elevation-1"
      @click:row="selectRow"
    >
    </v-data-table>
  </div>
</template>

<script lang="ts">
import { Vue, Component } from "nuxt-property-decorator";
import { getDatabase, ref, onValue } from "firebase/database";
@Component
export default class BookShelf extends Vue {
  tableHeader = [
    { text: "title", sortable: false, value: "title" },
    { text: "author", sortable: false, value: "author" }
  ];

  books = [];

  // 本の情報を全て取得する
  getBooks(this: any){
    let books: any;
    const db = getDatabase(this.$firebase);
    const data = ref(db, "books");
    onValue(data, (snapshot) => {
      books = snapshot.val();
    })
    for (const key in books) {
      const element = books[key];
      element['id'] = key;
      this.books.push(element);
    }
  }

  selectRow(data: any){
    this.$router.push("book/?id="+data.id);
  }

  // インスタンス初期化後に実行
  created() {
    this.getBooks();
  }
}
</script>

<style scope>
.tbl-col{
  font-size: 15px;
  padding-right: 23em;
}
</style>
