<template>
  <article class="card">
    <h3>{{item.product}}</h3>
    <div class="flex info">
      <img :src="`https://picsum.photos/seed/${item.id}/300`" alt />
      <div class="details">
        <p class="price">{{item.price}}</p>
        <p class="description">{{item.description}}</p>
        <p class="rating">Rating: {{item.rating.toFixed(1)}}</p>
        <div class="actions">
          <router-link v-if="item.stock>0" :to="`/item/${item.id}`" class="btn btn-clear btn-wide btn-red">Buy &raquo;</router-link>
          <div v-if="item.stock<1" class="alert">Out of Stock</div>
        </div>
      </div>
    </div>
  </article>
</template>

<script>
export default {
  name: "ItemCard",
  props: {
    item: Object
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.card {
  box-shadow: 0 0 16px 0 rgba(0, 0, 0, 0.15);
  padding: 1rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  h3 {
    font-size: xx-large;
  }
}
.info {
  justify-content: space-between;
  .price {
    font-size: x-large;
  }
  .alert {
    font-weight: 600;
    color: orange;
  }
  img {
    width: 50%;
    height: auto;
    object-fit: cover;
  }
  a {
    text-decoration: none;
  }
}
</style>
