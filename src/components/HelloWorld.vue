<template>
  <div class="d-flex justify-content-between align-items baseline p-2 my-2 bg-dark text-light">
    <a href="#" class="m-3" style="text-decoration: none; color:orange; font-size: 1.5rem;"
      @click="isBooksVisible = true">Books</a>
    <div class="d-flex align-items-baseline justify-content-between">
      <p class="mx-2">{{ list.items.length }} <template v-if="list.items.length == 1">item</template> <template
          v-else>items</template> | Total price [ {{ formatPrice(getTotalPrice()) }} ]</p>
      <button class="btn btn-primary m-3" @click="isBooksVisible = false">Show List</button>
    </div>
  </div>
  <!-- end of navbar-->


  <!--main content-->
  <div class="row m-auto text-center" v-if="isBooksVisible == true">
    <div class="card border m-2" style="width: 25rem;" v-for="book in books">
      <img :src="book.image" :alt="book.title">
      <div class="card-header" :title="book.author">
        {{ book.name }}
      </div>
      <ul class="list-group list-group-flush">
        <li class="list-group-item">
          <div class="d-flex align-items-baseline justify-content-between">
            <p class="mx-2">{{ book.category }}</p>
            <p class="mx-2">{{ book.author }}</p>
          </div>
        </li>
        <li class="list-group-item">
          <div class="d-flex align-items-baseline justify-content-between">
            <p :class="[book.pages > 50 ? 'more' : 'less']" class="mx-2">Total pages: {{ book.pages }}</p>
            <p class="mx-2">Price: {{ formatPrice(book.price) }}</p>
          </div>
        </li>
        <li class="list-group-item">
          <div class="d-flex align-items-baseline justify-content-between">
            <p class="mx-2">{{ book.isbn }}</p>
            <button href="#" class="btn btn-primary" @click="addToList(book)" :disabled="itemExistsInList(book)">Add To
              List</button>
          </div>
        </li>
      </ul>
    </div>
  </div>

  <div class="row text-center" v-if="isBooksVisible == false">
    <h3 class="text-danger" v-if="list.items.length == 0">
      There's no books to show in the list ...
    </h3>
    <table class="table table-striped table-bordered text-center" v-else>
      <thead>
        <tr>
          <th>Name</th>
          <th>Pages</th>
          <th>Author</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in list.items">
          <td>{{ item.book.name }}</td>
          <td>{{ item.book.pages }}</td>
          <td>{{ item.book.author }}</td>
          <td>
            <button class="btn btn-danger" @click="removeItem">Remove</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
  <!--end of main content-->


  <!--footer-->
  <div class="row">
    <div class="col-12 bg-dark text-light text-center p-3">
      <h6>Online Shopping app</h6>
      <p>2021</p>
    </div>
  </div>
  <!--end of footer-->
</template>

<script>
import books from '../books';
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data: () => ({
    books: books,
    isBooksVisible: true,
    list: {
      items: []
    }
  }),
  methods: {
    addToList(book) {
      this.list.items.push({
        book: book,
      });
    },
    getTotalPrice() {
      let total = 0;
      this.list.items.forEach(item => {
        total += item.book.price;
      });
      return total;
    },
    itemExistsInList(book) {
      return this.list.items.find(item => item.book.isbn === book.isbn);
    },
    removeItem(item) {
      this.list.items.splice(this.list.items.indexOf(item), 1);
    },
    formatPrice(price) {
      return Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD', minimumFractionDigits: 0 }).format(price);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
img {
  height: 35rem;
  object-fit: cover;
}

ul>li>div>p {
  padding: 0.6rem;
  font-size: 1.2rem;
  border-radius: 0.5rem;
  background-color: lightgrey;
}

.more {
  background-color: lightgreen;
}

.less {
  background-color: lightcoral;
}
</style>
