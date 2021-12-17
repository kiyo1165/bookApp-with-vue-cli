<template>
  <div>
      <v-row>
        <v-col cols="6">
          <v-text-field
            label="本のタイトルを検索"
            v-model="keyword"
          ></v-text-field>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="3">
          <v-btn depressed color="primary" @click="Saerch(keyword)">検索</v-btn>
        </v-col>
        <v-col cols="3">
          <v-btn color="secondary" to="/">ホームへ戻る</v-btn>
        </v-col>
      </v-row>
      <!-- カード -->
      <v-row>
        <v-col cols="12" md="4" v-for="(book, index) in saerchResults" :key="index">
          <v-card class="mx-auto" max-width="344">
            <v-img
              :src='book.image'
              height="200px"
            ></v-img>

            <v-card-title> {{ book.title }}</v-card-title>

            <v-card-subtitle> 1,000 miles of wonder </v-card-subtitle>

            <v-card-actions>
              <v-btn @click="addBookList(index)" color="orange lighten-2" text> 追加 </v-btn>

              <v-spacer></v-spacer>

              <v-btn icon @click="book.isActive = !book.isActive">
                <v-icon>{{
                  book.isActive ? "mdi-chevron-up" : "mdi-chevron-down"
                }}</v-icon>
              </v-btn>
            </v-card-actions>

            <v-expand-transition>
              <div v-show="book.isActive">
                <v-divider></v-divider>

                <v-card-text>
                  {{ book.description }}
                </v-card-text>
              </div>
            </v-expand-transition>
          </v-card>
        </v-col>
      </v-row>
  </div>
</template>

<script>
export default {
  name: "BookSearch",
  data() {
    return {
      keyword: "",
      saerchResults: [],
    };
  },
  methods: {
      addBookList(index){
          this.$emit('add-book-list', this.saerchResults[index])
      },
    //クエリストリングの作成
    async Saerch(keyword) {
      //初期化
      this.saerchResults = [];
      const baseUrl = "https://www.googleapis.com/books/v1/volumes?";
      //bookapi指定のパラメーター
      const params = {
        q: `intitle:${keyword}`,
        maxResults: 40,
      };
      //クエリストリングへ変換
      const queryParams = new URLSearchParams(params);
      console.log(baseUrl + queryParams);

      //fetchでjson取得
      const response = await fetch(baseUrl + queryParams)
        //responseをjsonに変換
        .then((response) => response.json())
        .then(console.log());
      for (let book of response.items) {
        //jsonオブジェクトのtitle,imageLinks, descriptionまでアクセス
        let title = book.volumeInfo.title;
        let img = book.volumeInfo.imageLinks ? book.volumeInfo.imageLinks : "";
        let description = book.volumeInfo.description;
        //必要な情報をpush, title情報なければ空を追加
        this.saerchResults.push({
          title: title ? title : "",
          image: img.thumbnail ? img.thumbnail : "",
          //40文字以内
          description: description ? description.slice(0, 40) : "",
          isActive: false
        });
      }
    },
  },
};
</script>

<style></style>
