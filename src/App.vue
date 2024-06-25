<template>
  <div
    class="w-4/5 m-auto h-[100vh] flex justify-center items-center flex-col gap-6"
  >
    <div
      class="box w-full text-center rounded-xl bg-cyan-600 overflow-idden px-3 py-8 relative min-h-48 flex justify-center items-center text-white lg:text-3xl md:text-2xl sm:text-xl text-lg"
    >
      <img class="imgs top-0 right-0 translate-x-1/4 -translate-y-1/4 rotate-180" src="./assets/quote.png" />
      <img class="imgs bottom-0 left-0 -translate-x-1/4 translate-y-1/4" src="./assets/quote.png" />
      {{ quote.content }}
      <span
        class="text-base sm:text-xl absolute text-white font-semibold bg-cyan-600 w-full text-end left-0 bottom-0 pr-2 pb-2 rounded-b-xl"
        >{{ quote.originator.name }}</span
      >
    </div>
    <ul
      class="flex justify-center w-full mx-auto relative flex-wrap gap-y-2 gap-x-3"
    >
      <li
        class="h-min bg-gradient-to-r from-cyan-500 to-blue-500 text-white px-2 py-1 rounded-3xl"
        v-for="tag in quote.tags"
        :key="tag"
      >
        {{ tag }}
      </li>
    </ul>
    <button
      @click="getQuote"
      class="bg-blue-600 px-2 py-1 rounded-xl text-white font-bold font-mono hover:shadow-2xl hover:bg-white hover:text-blue-600 border-blue-600 border-2 hover:rounded-none transition-all duration-300"
    >
      Get new quote
    </button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      quote: {
        content: "",
        originator: {
          name: "",
        },
      },
    };
  },
  mounted() {
    this.getQuote();
  },
  methods: {
    getQuote() {
      const xhr = new XMLHttpRequest();
      xhr.withCredentials = true;
      xhr.addEventListener("readystatechange", () => {
        if (xhr.readyState === 4) {
          if (xhr.status === 200) {
            try {
              this.quote = JSON.parse(xhr.responseText);
            } catch (error) {
              console.error("Error parsing response:", error);
            }
          } else {
            console.error("Error fetching quote:", xhr.statusText);
          }
        }
      });
      xhr.open(
        "GET",
        "https://quotes15.p.rapidapi.com/quotes/random/?language_code=en"
      );
      xhr.setRequestHeader(
        "x-rapidapi-key",
        "c8bc5eba24msh8516b52547225b4p1646dejsn673ae40d620e"
      );
      xhr.setRequestHeader("x-rapidapi-host", "quotes15.p.rapidapi.com");
      xhr.send();
    },
  },
};
</script>

<style scoped>
.imgs {
  @apply absolute w-16 z-10;
}
</style>
