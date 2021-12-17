<template>
  <div>
    BookEdit
    <!-- booksのidにアクセス -->
    <p>{{ book.title }}</p>
    <v-row>
      <v-col cols="12">
        <v-card class="max-auto">
          <v-row>
            <v-col cols="4">
              <v-img :src="book.image"></v-img>
            </v-col>
            <v-col cols="8">
              <v-card-title> タイトル：{{ book.title }} </v-card-title>
              読んだ日：
              <v-menu
                ref="menu"
                v-model="menu"
                :close-on-content-click="false"
                :return-value.sync="date"
                transition="scale-transition"
                offset-y
                min-width="auto"
              >
                <template v-slot:activator="{ on, attrs }">
                  <v-text-field
                    v-model="date"
                    label="Picker in menu"
                    prepend-icon="mdi-calendar"
                    readonly
                    v-bind="attrs"
                    v-on="on"
                  ></v-text-field>
                </template>
                <v-date-picker v-model="date" no-title scrollable>
                  <v-spacer></v-spacer>
                  <v-btn text color="primary" @click="menu = false">
                    Cancel
                  </v-btn>
                  <v-btn text color="primary" @click="$refs.menu.save(date)">
                    OK
                  </v-btn>
                </v-date-picker>
              </v-menu>
              感想：<v-textarea class="mx-2" v-model="book.memo"> </v-textarea>
              <v-card-actions>
                <v-btn color="secondary" to="/">一覧に戻る</v-btn>
                <v-btn color="info" @click="updateBookInfo">保存</v-btn>
                {{ book.memo }}
              </v-card-actions>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  name: "BookEdit",
  props: {
    books: Array,
  },
  data() {
    return {
      book: "",
      date: '',
      menu: false,
    };
  },
  beforeRouteEnter(to, from, next) {
    next((vm) => {
      //beforeRouteEnterはthisを使えないので代わりにvmがthisの役割を担う
      vm.$nextTick(() => {
        //Dom更新後に非同期処理を遅らせて実行させる
        vm.book = vm.books[vm.$route.params.id]; //booksのidをフックにオブジェクトを代入
        if(vm.book.readDate){
          vm.date = vm.book.readDate
        }else{
          vm.date = new Date().toISOString().substr(0, 10)
        }
        console.log(vm.book);
      });
    });
  },
  methods: {
    updateBookInfo() {
      this.$emit('update-book-info', {
        id: this.$route.params.id,
        readDate: this.date,
        memo: this.book.memo
      })
    },
  },
};
</script>

<style></style>
