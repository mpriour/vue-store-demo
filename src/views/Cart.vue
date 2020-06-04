<template>
  <div class="cart">
    <ul class="list list-plain">
      <li v-for="ci in cart" :key="ci.item.id" class="cart-item">
        <article class="flex">
          <img :src="`https://picsum.photos/seed/${ci.item.id}/300`" alt />
          <div class="flex info">
            <h3>{{ci.item.product}}</h3>
            <p class="cost">{{ci.item.price}} x {{ci.quantity}} =
              {{(ci.item.price.split('$')[1] * ci.quantity).toFixed(2)}}</p>
            <button class="btn btn-clear" @click="remove(ci)">Remove</button>
          </div>
        </article>
      </li>
    </ul>
  </div>
</template>

<script>
import store from "../store/memory";
export default {
  name: "Cart",
  data() {
    return {
      cart: []
    };
  },
  mounted() {
    const cart = store.get("cart");
    cart && (this.cart = JSON.parse(cart));
  },
  methods: {
    remove(cartItem) {
      this.cart = this.cart.filter(c => c.item.id != cartItem.item.id);
      store.replace("cart", this.cart);
      this.$root.$emit("cart:remove", cartItem);
    }
  }
};
</script>

<style lang="scss" scoped>
.cart{
  max-width: 600px;
  box-shadow: 0 0 16px 0 rgba(0, 0, 0, 0.15);
  padding: 1rem;
  display: flex;
  flex-direction: column;
  margin: 5vh auto;
  .list-plain{
    list-style: none;
  }
  .info {
    flex-direction: column;
    margin-left: 1rem;
  }
}
</style>
