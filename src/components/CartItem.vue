<script setup>
     import { defineProps, ref, watch } from 'vue';

     const props = defineProps({
          id: {
               type: Int32Array,
          },
          name: {
               type: String,
          },
          img: {
               type: String,
          },
          price: {
               type: Float32Array,
          },
          rating: {
               type: Float16Array,
          },
          quantity: {
               type: Int32Array,
          },
     })

     const quantity_ref = ref(props.quantity);

     

     watch(quantity_ref, () => {
          let ls = localStorage.getItem("ShopCart") ?? [];
          ls = ls.length > 0 ? JSON.parse(ls) : [];

          ls.forEach(el => {
               if(el.id == props.id) {
                    el.quantity = quantity_ref.value;
               }
          });

          localStorage.setItem("ShopCart", JSON.stringify(ls))

     })
</script>

<script>
     export default {
          methods: {
               UpdatePrice(id, n) {
                    this.$emit('change-quantity', id, n);
               }
          }
     }
</script>

<template>
     <div class="card" v-if="quantity_ref > 0">
          <div class="card__img-container">
               <img :src="img" alt="Изображение отсутствует" class="card__img">
          </div>
          <div class="card__info-container">
               <div class="card__info">
                    <p class="card__name">{{name}}</p>
                    <p class="card__price">{{price}} руб.</p>
                    <p class="rating">Рейтинг: {{ rating }}</p>
               </div>
               
               <div class="card__count-container">
                    <button class="card__count-btn" :disabled="quantity_ref <= 0" @click="quantity_ref--; UpdatePrice(id, -1)">-</button>
                    <p class="card__count">Количество: {{quantity_ref}}</p>
                    <button class="card__count-btn" @click="quantity_ref++; UpdatePrice(id, 1)">+</button>
               </div>
               <div class="card__buttons">
                    <RouterLink :to="{name: 'product', params:{productId: id}}"class="card__about">Подробнее</RouterLink>
                    <button @click="quantity_ref = 0; UpdatePrice(id, -quantity)" class="card__remove">Удалить из корзины</button>
               </div>
          </div>
     </div>
</template>

<style scoped>
     .card {
          width: 80%;
          min-width: 400px;
          height: 10vh;
          font-size: 14px;

          border: 1px solid black;
          border-radius: 15px;

          padding: 10px;
          box-sizing: border-box;

          display: flex;
          flex-direction: row;
          justify-content: space-between;
          align-items: center;

          background-color: burlywood;
     }

     .card__img-container {
          height: 100%;
          width: 5%;
          display: flex;
          flex-direction: row;
          align-items: center;
          justify-content: center;
          background-color: white;
          border-radius: 15px;
     }

     .card__img {
          width: 100%;
          height: 100%;
          /* width: 100%; */
          text-align: center;
          border-radius: 15px;
     }

     *::after, *::before {
          display: none;
     }

     .card__info-container {
          width: 100%;
          height: 100%;

          display: flex;
          flex-direction: row;
          justify-content: space-between;
          align-items: center;
     }


     .card__info-container p {
          margin: 0;
     }

     .card__info {
          width: 20%;
          display: flex;
          flex-direction: column;
          align-items: center;
     }

     .card__name {
          margin: 0;
          font-weight: bold;
          font-size: 1.7em;
          text-align: center;
     }

     .card__price {
          font-size: 1.5em;
     }

     .card__count-container {
          width: 30%;   
          display: flex;
          flex-direction: row;
          justify-content: space-around;
          align-items: center;
          font-size: 1.5em;
     }

     .card__count-btn {
          border-radius: 50%;
          height: 30px;
          width: 30px;
     }

     .card__buttons {
          width: 25vw;
          height: 100%;
          display: flex;
          flex-direction: column;
          justify-content: space-between;
     }

     .card__about {
          width: 100%;
          height: 45%;

          font-size: 1.5em;
          background-color: #ff4d36;
          color: black;
          text-decoration: none;

          display: flex;
          justify-content: center;
          align-items: center;

          border-radius: 15px;
     }

     .card__about:active {
          background-color: #e06363;
     }

     .card__remove {
          width: 100%;
          height: 45%;

          font-size: 1.5em;
          background-color: #6bf556;
          color: black;
          text-decoration: none;

          display: flex;
          justify-content: center;
          align-items: center;

          border: none;
          border-radius: 15px;
     }

     .card__remove:hover, .card__count-btn:hover {
          cursor: pointer;
     }

     .card__remove:active {
          background-color: #55cf42;
     }

     @media (max-width: 900px) {
          .card {
               width: 100%;
               font-size: 1.5vw;
          }

          .card__count-container {
               width: 50%;
          }
     }

     @media (max-width: 600px) {
          .card {
               width: 100%;
               font-size: 11px;
          }

          .card__remove {
               font-size: 1em;
          }

          .card__count-container {
               font-size: 1.2em;
          }
     }
</style>