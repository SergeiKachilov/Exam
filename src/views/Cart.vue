<script setup>
     import { ref, onMounted, defineProps, watch } from 'vue';
     import CartItem from '@/components/CartItem.vue';

     const ls = ref([]);
     const total_price = ref(0);

     ReadLocalStorage();
     
     function ReadLocalStorage() {
       ls.value = JSON.parse(localStorage.getItem("ShopCart"));
       return ls.value;
     //    ls.value = ls.value.length > 0 ? JSON.parse(ls.value) : [];
    }

    function GetTotalPrice() {
          ls.value.forEach(item => {
               total_price.value += +(item.price * item.quantity).toFixed(2);
          });
    }
     onMounted(async () => {
          ReadLocalStorage();
          GetTotalPrice();
          // ls = localStorage.getItem("ShopCart") ?? [];
          // cart_data.value = ls.length > 0 ? JSON.parse(ls) : [];
     })

     function UpdateTotalPrice(id, n) {
          // alert(`${id} ${n}`)

          // total_price.value = 0;
          ls.value.forEach(item => {
               if(item.id == id) {
                    item.quantity += n;
                    total_price.value += +(item.price * n).toFixed(2);
               }
          });
     }

     // watch(ls, () => {
     //      alert("Получилось")
     // })

</script>

<template>
     <div class="main-body">
          <div class="cart">
               <cart-item v-for="item in ls" @change-quantity="(id, n) => UpdateTotalPrice(id, n)"
               :id="item.id"
               :name="item.name"
               :img="item.img"
               :price="item.price"
               :rating="item.rating"
               :quantity="item.quantity"
               ></cart-item>
          </div>

          <p class="empty-cart" v-if="total_price.toFixed(2) == 0">Корзина пуста</p>

          <p class="total-price" v-if="total_price.toFixed(2) > 0">Общая сумма: {{ total_price.toFixed(2) }} руб.</p>
     </div>
</template>

<style scoped>
     .main-body {
          width: 100%;

          display: flex;
          flex-direction: column;
          align-items: center;
     }
     .cart {
          width: 100%;

          display: flex;
          flex-direction: column;
          align-items: center;
          justify-content: center;
          gap: 1vw;
     }

     .empty-cart {
          font-size: 32px;
     }

     .total-price {
          font-size: 32px;
     }

     @media (max-width: 900px) {
          .total-price {
               font-size: 24px;
          }
     }

     @media (max-width: 600px) {
          .total-price {
               font-size: 18px;
          }
     }
</style>