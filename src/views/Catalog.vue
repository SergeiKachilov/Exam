<script setup>
     import { onMounted, ref, watch } from 'vue';
     import Card from '@/components/Card.vue';

     const page = ref(1);
     const total_pages = ref(5);
     const data_page = ref(0);
     const categories_json = ref([]);
     const API_KEY = "8R2S3r3aR5KIb2zR";
     const active_category = ref(0);

     async function GetPage (page = 1, category = 0) {
          const response = await fetch(`https://nti.urfu.ru/api_exam/product?api_key=${API_KEY}&page=${page}&category_id=${category}`);
          data_page.value = await response.json();
          console.log(data_page.value)
     }

     async function GetCategories() {
          const response = await fetch(`https://nti.urfu.ru/api_exam/category?api_key=${API_KEY}`);
          categories_json.value = await response.json();
     }



     onMounted(async () => {
          await GetPage();
          await GetCategories();
     });

     watch(page, async () => {
          await GetPage(page.value, active_category.value);
     })

     watch(active_category, async () => {
          await GetPage(1, active_category.value);
     })
</script>

<template>
     <div class="main-body">
          <div class="categories">
               <button class="categories__btn" :disabled="active_category == 0" @click="active_category = 0">Все</button>
               <button class="categories__btn" v-for="category in categories_json" :disabled="active_category == category.id" @click="active_category = category.id">{{ category.name }}</button>
          </div>

          <div class="products">
               <card v-for="prod in data_page.items"
               :id="prod.id"
               :name="prod.name"
               :img="prod.img"
               :price="prod.price"
               :rating="prod.rating"
               ></card>
          </div>

          <div class="pagination">
               <button class="pagination__btn" :disabled="page <= 1" @click="page--"><= Назад</button>
               <p class="pagination__page">Страница {{ page }} из {{ data_page.total_pages }}</p>
               <button class="pagination__btn" :disabled="page >= data_page.total_pages" @click="page++">Вперед =></button>
          </div>
     </div>
</template>

<style scoped>
     .main-body {
          width: 100%;
          display: flex;
          flex-direction: column;
          align-items: center;
     }
     .products {
          width: 90%;
          display: flex;
          flex-direction: row;
          justify-content: space-around;
          flex-wrap: wrap;
          gap: 10px;
     }

     .categories {
          width: 60%;

          display: flex;
          flex-direction: row;
          justify-content: space-between;

          margin-bottom: 2vh;
     }

     .categories__btn {
          width: 24%;
          height: 2em;
          background-color: green;
          color: white;

          font-size: 18px;
     }

     

     .categories__btn:disabled {
          background-color: darkred;
     }

     .categories__btn:hover {
          cursor: pointer;
          background-color: darkgreen;
     }

     .categories__btn:disabled:hover {
          cursor: default;
          background-color: darkred;
     }

     .pagination {
          width: 25%;
          margin-top: 2vh;

          display: flex;
          flex-direction: row;
          justify-content: space-between;
          align-items: center;
     }

     .pagination__btn {
          width: 7em;
          height: 3vh;
          font-size: 1vw;
     }

     .pagination__page {
          font-size: 1vw;
     }

     @media (max-width: 900px) {
          .categories__btn {
               font-size: 1.5vw;
          }

          .pagination {
               width: 40%;
          }

          .pagination__btn, .pagination__page {
               font-size: 1.5vw;
          }
     }

     @media (max-width: 700px) {
          .categories, .products {
               width: 100%;
          }

          .categories__btn {
               font-size: 12px;
               justify-content: space-around;
          }

          .pagination {
               width: 60%;
               justify-content: space-around;
          }

          .pagination__btn, .pagination__page {
               font-size: 12px;
          }
     }

     @media (max-width: 500px) {
          .pagination {
               width: 100%;
          }
     }
</style>