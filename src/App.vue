<script setup>
  import { RouterLink, RouterView } from 'vue-router'
  import { onMounted, ref } from 'vue';

  const ls = ref([]);

  function ReadLocalStorage() {
        ls.value = localStorage.getItem("ShopCart") ?? [];
        ls.value = ls.value.length > 0 ? JSON.parse(ls.value) : [];
    }

  onMounted(async () => {
      ReadLocalStorage();
      // ls = localStorage.getItem("ShopCart") ?? [];
      // cart_data.value = ls.length > 0 ? JSON.parse(ls) : [];
  })

</script>

<template>
  <header class="header">
    <p class="header__logo">Shop</p>
    <div class="header__buttons">
      <RouterLink to="/" class="header__btn">Каталог</RouterLink>
      <RouterLink to="/cart" class="header__btn" @click="ReadLocalStorage()" :cart_json="ls.value">Корзина</RouterLink>
    </div>
  </header>

  <RouterView />
</template>

<style scoped>
  .header {
    width: 100%;
    height: 4vw;
    min-height: 33px;

    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;

    background-color: gray;
    padding: 0 20px;
    margin-bottom: 2vh;
    box-sizing: border-box;
  }

  .header__logo {
    color: white;
    font-size: 3vw;
    width: 30%;

  }

  .header__logo:hover {
    cursor: default;
  }

  .header__buttons {
    width: 70%;

    display: flex;
    flex-direction: row;
    justify-content: space-around;
    font-size: 2vw;
  }

  .header__btn {
    color: white;
    text-decoration: none;
  }

  .header__btn:hover {
    color: purple
  }

  @media (max-width: 600px) {
    .header__btn {
      font-size: 18px;
    }

    .header__logo {
      font-size: 20px;
    }
  }
</style>
