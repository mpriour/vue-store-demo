<template>
  <div class="card-container">
    <item-card v-for="item in items" :item="item" :key="item.id"/>
  </div>
</template>

<script>
// @ is an alias to /src
import ItemCard from '@/components/ItemCard.vue'
import store from '../store/memory'

export default {
  name: 'Home',
  components: {
    ItemCard
  },
  data(){
    return {
      items: []
    }
  },
  async mounted(){
    this.items = await fetch('./data/inventory.json').then(r=>r.json())
    this.items.forEach(item=>{
      store.save(item.id,JSON.stringify(item))
    })
  }
}
</script>

<style lang="scss" scoped>
.card-container{
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  column-gap: 20px;
  row-gap: 20px;
}
</style>
