<script setup>
     import { onMounted, ref } from 'vue';
     import { useRoute } from 'vue-router';

     const product_info = ref([]);
     const route = useRoute();
     const API_KEY = "8R2S3r3aR5KIb2zR";

     async function GetInfo(id) {
          const response = await fetch(`https://nti.urfu.ru/api_exam/product/${id}?api_key=${API_KEY}`);
          product_info.value = await response.json();
          console.log(product_info.value);
     }

     function AddToCart() {
          let ls = localStorage.getItem("ShopCart") ?? [];
          ls = ls.length > 0 ? JSON.parse(ls) : [];
          let flag = false;

          ls.forEach(el => {
               if(el.id == product_info.value.id) {
                    el.quantity++;
                    flag = true;
               }
          })

          if (!flag) {
               let product = {
                    id: product_info.value.id,
                    name: product_info.value.name,
                    img: product_info.value.img,
                    price: product_info.value.price,
                    rating: product_info.value.rating,
                    quantity: 1
               }
               ls.push(product);
          }

          localStorage.setItem("ShopCart", JSON.stringify(ls));
     }

     onMounted(async () => {
          GetInfo(route.params.productId);
     })
</script>

<template>
     <div class="main-body">
          <div class="container">
               <img :src="product_info.img" alt="Изображение отсутствует" class="container__img">
               <p class="container__price">{{product_info.price}} руб.</p>
          </div>
          <div class="info">
               <p class="info__name">Название: <strong>{{product_info.name}}</strong></p>
               <p class="info__rating">Рейтинг: {{product_info.rating}} / 5</p>
               <p class="info__rating-count">Всего оценок: {{ product_info.rating_count }}</p>
               <p class="info__description">Описание: {{ product_info.description ?? "отсутствует" }}</p>
               <button class="info__cart" @click="AddToCart()">Добавить в корзину</button>
          </div>
     </div>
</template>

<style scoped>
     .main-body {
          width: 100%;

          display: flex;
          flex-direction: row;
          justify-content: center;
          flex-wrap: wrap;
          box-sizing: border-box;

          font-size: 28px;
          gap: 10px;
     }

     .container {
          width: 300px;
          height: 40vh;
          /* min-width: 200px; */
          display: flex;
          flex-direction: column;
          justify-content: space-between;
          align-items: center;
          box-sizing: border-box;
          background-color: #ff9924;
          border-radius: 15px;
          padding: 20px;
          box-sizing: border-box;
     }

     .container__img {
          width: 100%;
          height: 80%;
          border-radius: 15px;
     }

     .container__price {
          font-size: 1.1em;
          font-weight: bold;
          margin: 0;
     }

     .info {
          width: 40%;
          padding: 20px;
          box-sizing: border-box;
          min-width: 200px;
          display: flex;
          flex-direction: column;
          vertical-align: top;
          gap: 1vw;
          justify-content: center;
          background-color: burlywood;
          border-radius: 15px;
          align-items: center;
     }

     .info p {
          margin: 0;
          width: 100%;
     }

     .info__cart {
          width: 70%;
          height: 20%;
          font-weight: bold;
          font-size: 0.8em;
          background-color: #6bf556;
          color: black;
          text-decoration: none;

          display: flex;
          justify-content: center;
          align-items: center;

          border: none;
          border-radius: 15px;
          margin-top: 5%;
     }

     .info__cart:hover {
          cursor: pointer;
     }

     .info__cart:active {
          background-color: #55cf42;
     }

     @media (max-width: 1000px) {
          .main-body {
               font-size: 24px;
          }
          .container {
               width: 200px;
               height: 30vh;
          }
     }

     @media (max-width: 700px) {
          .main-body {
               font-size: 18px;
          }
     }

     @media (max-width: 700px) {
          .container {
               width: 200px;
          }

          .info {
               width: 300px;
          }
          
     }
</style>