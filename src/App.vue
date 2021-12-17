<template>
  <v-app>
    <Header />

    <v-main>
      <!-- 全てのコンテナで使うのでrouter-viewを挟む -->
      <v-container>
        <!-- BookEditへ渡すbook props -->
        <router-view
        :books="books"
        @add-book-list="addBook"
        @update-book-info="updateBookInfo" />
      </v-container>
    </v-main>
    <Footer />
  </v-app>
</template>

<script>
import Header from "@/global/Header";
import Footer from "@/global/Footer";
const STORAGE_KEY = "books";
export default {
  name: "App",
  components: {
    Header,
    Footer,
  },
  data() {
    return {
      books: [],
      newBook: null,
    };
  },
  mounted() {
    if (localStorage.getItem(STORAGE_KEY)) {
      try {
        this.books = JSON.parse(localStorage.getItem(STORAGE_KEY)); //JSON.parse: json ->文字列
      } catch (e) {
        localStorage.removeItem(STORAGE_KEY);
      }
    }
  },
  methods: {
    addBook(e) { //e引数は子供から（@add-book-list）送られてくるデータ
      console.log(e)
      this.books.push({
        id: this.books.length,
        title: e.title,
        image: e.image,
        description: e.description,
        readDate: '',
        memo: '',
        isActive: false
      });
      // this.newBook = "";
      this.saveBooks();
      //最後に追加したidの取得
      let _id = this.books.slice(-1)[0].id
      //編集ページへ遷移
      this.toEdit(_id)
    },
    removeBook(x) {
      this.books.splice(x, 1);
      this.saveBooks();
    },
    saveBooks() {
      const parsed = JSON.stringify(this.books); //文字列 ->json
      localStorage.setItem(STORAGE_KEY, parsed);
    },
    updateBookInfo(e){
      const params = { //spliceに置き換えるオブジェクトの作成
        id: e.id,
        readDate: e.readDate,
        memo: e.memo,
        title: this.books[e.id].title,
        description: this.books[e.id].description,
        image: this.books[e.id].image,
        isActive: false
      }
      this.books.splice(e.id, 1, params)
      this.saveBooks()
      this.$router.push('/') //toppageに遷移
    },
    toEdit(_id){
      this.$router.push(`/edit/${_id}`)

    }
  },
};
</script>
