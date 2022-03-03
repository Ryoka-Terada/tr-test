<template>
  <div>
    <v-simple-table fixed-header>
      <thead>
        <th class="tbl-col">title</th>
        <th class="tbl-col">author</th>
      </thead>
      <tbody>
        <tr
          v-for="(book, i) in books"
          :key="i"
          @click="selectRow(i)"
        >
          <td>{{ book.title }}</td>
          <td>{{ book.author }}</td>
        </tr>
      </tbody>
    </v-simple-table>
  </div>
</template>

<script lang="ts">
import { Vue, Component } from "nuxt-property-decorator";
import { getDatabase, ref, onValue } from "firebase/database";
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

  selectRow(id: String){
    this.$router.push("/book?id="+id);
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
