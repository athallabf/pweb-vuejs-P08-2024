<script lang="ts">
import BookCard from "../components/BookCard.vue";

interface BookObject {
  _id: string;
  title: string;
  author: string;
  tags: string[];
  publishedDate: string;
  initialQty: number;
  qty: number;
  publisher: string;
}

export default {
  name: "HomeView",
  data: () => ({
    booksData: [] as BookObject[],
    fetchError: false,
  }),
  async mounted() {
    try {
      const response = await fetch("http://localhost:8080/book");
      if (!response.ok) {
        throw new Error("Failed to fetch books data");
      }
      const data = await response.json();
      console.log(data);
      this.booksData = [...data.data];
    } catch (error) {
      console.error(error);
      this.fetchError = true;
    }
  },
  components: {
    BookCard,
  },
};
</script>

<template>
  <main class="h-full bg-white">
    <!-- Hero Section -->
    <section class="hero min-h-screen bg-blue-500 text-white py-16 text-center px-[100px] py-[36px]">
      <h1 class="text-4xl font-bold">Welcome to Book Gallery App P8</h1>
      <p class="text-lg mt-4">Discover amazing books from various authors!</p>
      <button class="mt-6 px-4 py-2 bg-white text-blue-500 font-semibold rounded shadow">
        Explore Now
      </button>
    </section>

    <!-- About Section -->
    <section class="about min-h-screen bg-gray-100 py-16 px-4">
      <h2 class="text-3xl font-bold text-center">About Us</h2>
      <p class="text-center text-lg mt-4">
        We are passionate about books and aim to provide the best collection for
        book lovers everywhere.
      </p>
    </section>

    <!-- Books Section -->
    <section class="books min-h-screen mt-10 px-4">
      <h2 class="text-3xl font-bold text-center text-black">Our Book Collection</h2>
      <div class="flex flex-wrap justify-center gap-4 mt-10">
        <BookCard
          v-if="booksData.length"
          v-for="book in booksData"
          :key="book._id"
          :book="book"
          class="w-full sm:w-full md:w-2/6 lg:w-1/4"
        />
        <h1
          class="font-bold text-3xl text-center w-full"
          v-else-if="fetchError"
        >
          Failed to load books
        </h1>
        <h1 class="font-bold text-3xl text-center w-full" v-else>Loading...</h1>
      </div>
    </section>

    <!-- Footer Section -->
    <footer class="footer bg-blue-500 text-white py-4 text-center">
      <p>&copy; 2024 Book Gallery App P8. All rights reserved.</p>
    </footer>
  </main>
</template>