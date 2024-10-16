<script setup>
import { computed } from "vue";

const props = defineProps({
  cart: {
    type: Array,
    required: true,
  },
  guitar: {
    type: Object,
    required: true,
  },
});

defineEmits([
  "decrease-item-quantity",
  "increase-item-quantity",
  "remove-item",
  "empty-cart",
  "add-to-cart",
]);

const totalPrice = computed(() => {
  return props.cart.reduce(
    (total, item) => total + item.quantity * item.price,
    0
  );
});
</script>

<template>
  <header class="py-5 header">
    <div class="container-xl">
      <div class="row justify-content-center justify-content-md-between">
        <div class="col-8 col-md-3">
          <a href="index.html">
            <img class="img-fluid" src="/img/logo.svg" alt="imagen logo" />
          </a>
        </div>
        <nav
          class="col-md-6 a mt-5 d-flex align-items-start justify-content-end"
        >
          <div class="carrito">
            <img
              class="img-fluid"
              src="/img/carrito.png"
              alt="imagen carrito"
            />

            <div id="carrito" class="bg-white p-3">
              <p v-if="cart.length === 0" class="text-center m-0">
                Cart is Empty
              </p>
              <div v-else>
                <table class="w-100 table">
                  <thead>
                    <tr>
                      <th>Image</th>
                      <th>Name</th>
                      <th>Price</th>
                      <th>Quantity</th>
                      <th></th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="item in cart">
                      <td>
                        <img
                          class="img-fluid"
                          :src="'/img/' + item.image + '.jpg'"
                          :alt="'guitar image ' + item.name"
                        />
                      </td>
                      <td>{{ item.name }}</td>
                      <td class="fw-bold">${{ item.price }}</td>
                      <td class="flex align-items-start gap-4">
                        <button
                          type="button"
                          class="btn btn-dark"
                          @click="$emit('decrease-item-quantity', item.id)"
                        >
                          -
                        </button>
                        {{ item.quantity }}
                        <button
                          type="button"
                          class="btn btn-dark"
                          @click="$emit('increase-item-quantity', item.id)"
                        >
                          +
                        </button>
                      </td>
                      <td>
                        <button
                          class="btn btn-danger"
                          type="button"
                          @click="$emit('remove-item', item.id)"
                        >
                          X
                        </button>
                      </td>
                    </tr>
                  </tbody>
                </table>

                <p class="text-end">
                  Total: <span class="fw-bold">${{ totalPrice }}</span>
                </p>
                <button
                  class="btn btn-dark w-100 mt-3 p-2"
                  @click="$emit('empty-cart')"
                >
                  Empty Cart
                </button>
              </div>
            </div>
          </div>
        </nav>
      </div>
      <!--.row-->

      <div class="row mt-5">
        <div class="col-md-6 text-center text-md-start pt-5">
          <h1 class="display-2 fw-bold">{{ guitar.name }} Model</h1>
          <p class="mt-5 fs-5 text-white">{{ guitar.description }}</p>
          <p class="text-primary fs-1 fw-black">${{ guitar.price }}</p>
          <button
            type="button"
            class="btn fs-4 bg-primary text-white py-2 px-5"
            @click="$emit('add-to-cart', guitar)"
          >
            Add to Cart
          </button>
        </div>
      </div>
    </div>

    <img
      class="header-guitarra"
      src="/img/header_guitarra.png"
      alt="imagen header"
    />
  </header>
</template>

<style lang="css" scoped></style>
