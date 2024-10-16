<script setup>
import { ref, onMounted, watch } from "vue";
import { db } from "./data/guitars";
import Guitar from "./components/Guitar.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";

let guitars = ref([]);
const cart = ref([]);
const guitar = ref({});

watch(
  cart,
  () => {
    saveToLocalStorage();
  },
  {
    deep: true,
  }
);

onMounted(() => {
  guitars.value = db;
  guitar.value = db[3];

  const cartStorage = localStorage.getItem("cart");

  if (cartStorage) {
    cart.value = JSON.parse(cartStorage);
  }
});

const saveToLocalStorage = () => {
  localStorage.setItem("cart", JSON.stringify(cart.value));
};

const addToCart = (guitar) => {
  const itemIndex = cart.value.findIndex((item) => item.id === guitar.id);

  if (itemIndex >= 0) {
    cart.value[itemIndex].quantity++;
  } else {
    guitar.quantity = 1;
    cart.value.push(guitar);
  }
};

const decreaseItemQuantity = (id) => {
  const index = cart.value.findIndex((item) => item.id === id);
  if (cart.value[index].quantity <= 1) return;
  cart.value[index].quantity--;
};

const increaseItemQuantity = (id) => {
  const index = cart.value.findIndex((item) => item.id === id);
  if (cart.value[index].quantity >= 5) return;
  cart.value[index].quantity++;
};

const removeItem = (id) => {
  cart.value = cart.value.filter((item) => item.id !== id);
};

const emptyCart = () => {
  cart.value = [];
};
</script>

<template>
  <Header
    :cart="cart"
    :guitar="guitar"
    @decrease-item-quantity="decreaseItemQuantity"
    @increase-item-quantity="increaseItemQuantity"
    @remove-item="removeItem"
    @empty-cart="emptyCart"
    @add-to-cart="addToCart"
  />
  <main class="container-xl mt-5">
    <h2 class="text-center">Our Collection</h2>

    <div class="row mt-5">
      <Guitar
        v-for="guitar in guitars"
        :guitar="guitar"
        @add-to-cart="addToCart"
      />
    </div>
  </main>
  <Footer />
</template>
