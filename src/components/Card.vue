<script setup>
     import { defineProps, ref, onMounted } from 'vue';
     import { RouterLink } from 'vue-router';
     // const id_prod = ref(0);
     defineProps({
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
     })

     const cart_json = ref([]);

     function AddToCart(id_prod, name_prod, img_prod, price_prod, rating_prod) {

          cart_json.value = localStorage.getItem("ShopCart") ?? [];
          let flag = false;
          cart_json.value = cart_json.value.length > 0 ? JSON.parse(cart_json.value) : [];
          // cart.value.slice(0,-1);
          cart_json.value.forEach(element => {
               if (element.id == id_prod) {
                    element.quantity = +element.quantity + 1;
                    flag = true;
               }
          });

          if (!flag) {
               let product = {
                    id: id_prod,
                    name: name_prod,
                    img: img_prod,
                    price: price_prod,
                    rating: rating_prod,
                    quantity: 1
               }
               cart_json.value.push(product);    
          }

          cart_json.value = JSON.stringify(cart_json.value);
          localStorage.setItem("ShopCart", cart_json.value);

     }

     onMounted (async () => {
          cart_json.value = localStorage.getItem("ShopCart") ?? [];
          // let product = {
          //      id: 0,
          //      count: 1
          // }
          // cart.value.push(product)
     })
</script>

<template>
     <div class="card" @click="console.log(id)">
          <div class="card__img-container"><img :src="img" alt="Изображение отсутствует" class="card__img"></div>
          <p class="card__name">{{ name }}</p>
          <p class="card__price">{{ price }} руб.</p>
          <p class="card__rating">Рейтинг: {{ rating }}</p>
          <div class="card__buttons">
               <RouterLink :to="{name: 'product', params:{productId: id}}"class="card__about">Подробнее</RouterLink>
               <button class="card__btn" @click="AddToCart(id, name, img, price, rating)">Добавить в корзину</button>
          </div>
     </div>
</template>

<style scoped>
     .card {
          width: 5vw;
          min-width: 300px;
          height: 50vh;

          border: 1px solid black;
          border-radius: 15px;

          padding: 10px;
          box-sizing: border-box;

          display: flex;
          flex-direction: column;
          justify-content: space-between;
          align-items: center;

          background-color: #ff9924;

     }

     .card p {
          margin: 0;
     }

     .card__img-container {
          height: 60%;
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
          border-radius: 15px;
     }

     .card__name {
          font-size: 28px;
     }

     .card__price {
          font-size: 24px;
     }

     .card__buttons {
          width: 80%;
          height: 20%;

          display: flex;
          flex-direction: column;
          gap: 10px;
          align-items: center;
          justify-content: space-between;
     }

     .card__about {
          width: 100%;
          height: 45%;

          font-size: 24px;
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

     .card__btn {
          width: 100%;
          height: 45%;

          font-size: 18px;
          background-color: #6bf556;
          color: black;
          text-decoration: none;

          display: flex;
          justify-content: center;
          align-items: center;

          border: none;
          border-radius: 15px;
     }

     .card__btn:hover {
          cursor: pointer;
     }

     .card__btn:active {
          background-color: #55cf42;
     }
</style>