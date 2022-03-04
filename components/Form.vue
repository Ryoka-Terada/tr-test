<template>
  <v-card>
    <v-card-title>
      {{ pagetitle }}
      <v-spacer />
      <v-btn v-if="!input" fab small color="accent" elevation="5" @click="modeChange">
        <v-icon v-if="read" color="primary">
          mdi-pencil
        </v-icon>
        <v-icon v-if="edit" color="primary">
          mdi-lock
        </v-icon>
      </v-btn>
    </v-card-title>
    <v-form ref="bookForm">
      <v-card-actions>
        <v-text-field
          v-model="bookTitle"
          placeholder="本の題名"
          :rules="[required]"
          :disabled=read
        ></v-text-field>
      </v-card-actions>
      <v-card-actions>
        <v-text-field
          v-model="bookAuthor"
          placeholder="作者"
          :rules="[required]"
          :disabled=read
        ></v-text-field>
      </v-card-actions>
      <v-card-actions>
        <v-textarea
          v-model="bookPassage"
          placeholder="印象的だった文"
          :rules="[required, maxCount]"
          counter="100"
          :disabled=read
        ></v-textarea>
      </v-card-actions>
    </v-form>
    <v-card-actions>
      <v-btn v-if="edit" color="error" @click="deleteBook(id)">削除</v-btn>
      <v-spacer />
      <v-btn v-if="edit&!isChange" disabled>再提出</v-btn>
      <v-btn v-if="edit&isChange" color="primary" @click="setBook(bookTitle, bookAuthor, bookPassage)">再提出</v-btn>
      <v-btn v-if="input" color="primary" @click="pushBook(bookTitle, bookAuthor, bookPassage)">提出</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script lang="ts">
import { Vue, Component, Prop, Watch } from "nuxt-property-decorator";
import { getDatabase, ref, onValue, set, push, remove } from "firebase/database";
@Component
export default class BookShelf extends Vue {
  @Prop({ type: String, required: true })
  pagetitle: String;

  @Prop({ type: Boolean, required: false })
  read: Boolean;

  @Prop({ type: Boolean, required: false })
  input: Boolean;

  @Prop({ type: Boolean, required: false })
  edit: Boolean;

  @Prop({ type: String, required: false })
  id: String;

  isChange: Boolean = false;
  bookTitle: String = "";
  bookAuthor: String = "";
  bookPassage: String = "";

  // インスタンス初期化後に実行
  // 閲覧モードの場合は本の情報を取得
  created(){
    if(this.read){
      this.getBook(this.id);
    }
  }

  // バリデーション(必須)を定義
  required = (value: String) => {
    if(value==="") return "必須入力です"
  };

  // バリデーション(最大文字数)を定義
  maxCount = (value: String) => {
    if(value.length>100) return "100文字以下で入力してください"
  };

  // 編集モードの際、項目に変更が加わった場合に再提出ボタンを活性にする
  @Watch('bookTitle',{deep: true})
  @Watch('bookAuthor',{deep: true})
  @Watch('bookPassage',{deep: true})
  changeBookInfo(){
    if(this.edit){
      this.isChange = true;
    }
  }

  // 詳細情報を取得
  getBook(this: any, id: String){
    const db = getDatabase(this.$firebase);
    const data = ref(db, "books/"+id);
    onValue(data, (snapshot) => {
      const book = snapshot.val();
      this.bookTitle = book.title;
      this.bookAuthor = book.author;
      this.bookPassage = book.passage;
    })
  }

  // 本の情報を登録する(IDは自動採番される)
  pushBook(this: any, bookTitle: string, bookAuthor: string, bookPassage:string){
    if(this.$refs.bookForm.validate()){
      const db = getDatabase(this.$firebase);
      const data = ref(db, "books");
      const val = {
        title: bookTitle,
        author: bookAuthor,
        passage: bookPassage
      };
      push(data, val);
      // 入力フォームを全てクリア
      this.bookTitle = "";
      this.bookAuthor = "";
      this.bookPassage = "";
      // バリデーションチェックもクリア
      this.$refs.bookForm.resetValidation();
    }
  }

  // 本の情報を更新する
  setBook(this: any, bookTitle: string, bookAuthor: string, bookPassage:string){
    if(this.$refs.bookForm.validate()){
      const db = getDatabase(this.$firebase);
      const data = ref(db, "books/"+this.id);
      const val = {
        title: bookTitle,
        author: bookAuthor,
        passage: bookPassage
      }
      set(data, val);
    }
    // 更新後は閲覧モードに戻す
    this.modeChange();
  }

  // 本の情報を削除する
  deleteBook(this: any, id: String){
    const db = getDatabase(this.$firebase);
    const data = ref(db, "books/"+id);
    remove(data);
    // 削除後、一覧に遷移
    this.$router.push("/bookList");
  }

  // 閲覧モードと編集モードを切り替える
  modeChange(){
    this.read = !this.read;
    this.edit = !this.edit;
  }

}
</script>
