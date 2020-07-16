<template>
    <div>
        <h4>Best Sellers</h4>
        <div v-if="bestsellers && bestsellers.length > 0">
            <form v-on:submit.prevent="addBestseller" class="" action="#" method="post">
                <select class="bestsellers" name="bestsellers" @change="showBookImage">
                    <option v-for="item in bestsellers">{{item.title}} by {{item.author}}</option>
                </select>
                <button type="submit" name="button">Add Book</button>
            </form>
        </div>
        <div v-else>Loading titles...</div>
    </div>
</template>

<script>
export default {
  name: 'BestSellerForm',
  props: [],
  data() {
    return {
        bestsellers: []
    };
  },
  methods: {
        showBookImage(event) {
            var bookIndex = event.target.selectedIndex;
            var book = this.bestsellers[bookIndex];
            console.log("load image - " + book.book_image);
        },
        addBestseller(event) {
            var bookIndex = event.target.elements.bestsellers.selectedIndex;
            var book = this.bestsellers[bookIndex];
            console.log(book);
        }
  },
  mounted: function () {
    var apiPrefix = "https://api.nytimes.com/svc/books/v3/lists/current/";
    var apiKey = "FS7LArfy6VoB0xovLFSanPm0VssVnAjn";
    var bookForm = "hardcover";
    var bookType = "fiction";
    var apiURL =  apiPrefix + bookForm + "-" + bookType + ".json?api-key=" + apiKey;
    this.axios
        .get(apiURL)
        .then(response => {
            var books = response.data.results.books;
            this.bestsellers = books;
        })
  },
};
</script>

<style>
SELECT.bestsellers {margin:auto; display:block;}
</style>
