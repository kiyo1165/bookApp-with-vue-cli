<template>
  <div>
    <h1>本一覧</h1>
    <div v-if="!books.length">
      <p>まだ登録がありません。</p>
    </div>
    <v-row align="center" justify="space-around">
      <v-btn depressed @click="toSearch"> 本の検索 </v-btn>
    </v-row>
    <v-row>
      <v-col cols="12" md="4" v-for="(book, index) in books" :key="index">
        <v-card class="mx-auto" max-width="344">
          <v-img :src="book.image" height="200px"></v-img>

          <v-card-title> {{ book.title }} </v-card-title>

          <v-card-subtitle>
            メモ：{{ book.memo }} <br />
            読んだ日：{{ book.readDate }}
          </v-card-subtitle>
          <v-row justify="start">
              <v-col cols="12">
                   <v-card-actions>
                    <v-btn elevation="2" small @click="toEdit(book.id)">編集 </v-btn>
               </v-card-actions>
              </v-col>
                 </v-row>
            <v-card-actions>
              <v-spacer></v-spacer>
             

              <v-btn icon @click="book.isActive = !book.isActive">
                <v-icon>{{
                  book.isAcitive ? "mdi-chevron-up" : "mdi-chevron-down"
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
  props: {
    books: Array,
  },
  methods: {
    toSearch() {
      console.log(this.$router);
      this.$router.push("/search");
    },
    toEdit(id) {
      this.$router.push(`edit/${id}`);
    },
  },
};
</script>

<style></style>
