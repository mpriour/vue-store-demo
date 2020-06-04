<template>
  <article class="card" v-if="item">
    <img :src="`https://picsum.photos/seed/${item.id}/300`" alt />
    <div class="flex info">
      <h3>{{item.product}}</h3>
      <p class="description">{{item.description}}</p>
      <p class="rating">Rating: {{item.rating.toFixed(1)}}</p>
    </div>
    <div class="flex actions">
      <p class="price">{{item.price}}</p>
      <button class="btn btn-clear" @click="addCart(item)">Add to Cart</button>
      <p v-if="item.stock>0">Amount Left: {{item.stock}}</p>
      <p v-else class="alert">Out of Stock</p>
      <!-- <button class="btn btn-link btn-small">Edit</button> -->
    </div>
  </article>
  <article v-else>
    <h3>Product {{id}} not found</h3>
  </article>
</template>

<style lang="scss" scoped>
.card {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  .info, .actions {
    flex-direction: column;
  }
}
</style>

<script>
import store from "../store/memory";
export default {
  name: "Item",
  props: {
    id: String
  },
  data() {
    return {
      item: null
    };
  },
  watch: {
    id(newId) {
      const item = JSON.parse(store.get(newId));
      item && (this.item = item);
    }
  },
  mounted() {
    const item = JSON.parse(store.get(this.id));
    item && (this.item = item);
  },
  methods: {
    addCart(item){
      this.item.stock = Math.max(this.item.stock - 1, 0)
      store.replace(this.id, this.item)
      this.$root.$emit('cart:add', item)
    }
  }
};
</script>
