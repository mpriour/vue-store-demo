<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link>|
      <router-link to="/cart">Cart</router-link>
    </div>
    <router-view />
  </div>
</template>

<script>
import store from './store/memory'
export default {
  mounted(){
    this.$root.$on('cart:add', item => {
      const cart = JSON.parse(store.get('cart'))
      if(!cart){
        store.save('cart', JSON.stringify([{quantity:1,item}]))
      } else {
        //find the item, add if not exists, increment if it exists
        let cartIndex = -1
        cart.some((ci, ndx) => {
          if(ci.item.id == item.id){
            cartIndex = ndx
            return true
          }
        })
        if(cartIndex == -1){
          cart.push({item, quantity:1})
        } else {
          cart[cartIndex].quantity = cart[cartIndex].quantity + 1
        }
        store.replace('cart', JSON.stringify(cart))
      }
    })
    this.$root.$on('cart:remove', cartItem => {
      //find the item in the store and add the quantity back to it
      const item = JSON.parse(store.get(cartItem.item.id))
      if(item){
        item.stock = item.stock + cartItem.quantity
        store.replace(item.id, JSON.stringify(item))
      }
    })
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.flex {
  display: flex;
}
#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
.btn {
  position: relative;
  display: inline-block;
  padding: 0.375rem 1rem;
  width: auto;
  color: #ffffff;
  border: 1px solid #0079c1;
  border-radius: 0;
  background-color: #0079c1;
  box-sizing: border-box;
  transition: all 0.05s linear;
  cursor: pointer;
  font-size: 0.9375rem;
  line-height: 1.5;
}
.btn:hover {
  text-decoration: none;
  color: #ffffff;
  background-color: #005e95;
  border-color: #005e95;
}

.btn-transparent {
  background: none;
  color: #0079c1;
  border: none;
}
.btn-transparent:hover {
  color: #005e95;
  background: none;
  text-decoration: underline;
}

.btn-clear {
  color: #0079c1;
  background: #ffffff;
  border-color: #0079c1;
}
.btn-clear:hover {
  color: #ffffff;
  background: #005e95;
  border-color: #005e95;
}

.btn-clear-white {
  background: none;
  color: #ffffff;
  border: 1px solid #ffffff;
}
.btn-clear-white:hover {
  color: #959595;
  background: #ffffff;
  border-color: #ffffff;
}

.btn-small {
  font-size: 0.875rem;
  line-height: 1.5;
  padding: 0.28125rem 0.75rem;
}

.btn-large {
  font-size: 1rem;
  line-height: 1.5;
  padding: 0.5rem 1rem;
}

.btn-fill {
  width: 100%;
  text-align: center;
}

.btn-half {
  width: 50%;
  text-align: center;
}

.btn-grouped {
  float: left;
}
html[dir="rtl"] .btn-grouped {
  float: right;
}
.btn-grouped + .btn-grouped {
  margin-left: 1px;
}
html[dir="rtl"] .btn-grouped + .btn-grouped {
  margin-left: 0;
  margin-right: 1px;
}
.btn-grouped:last-of-type {
  float: none;
}
.btn-grouped.btn-half {
  margin-left: 0;
}

.btn-white {
  background: #ffffff;
  color: #4c4c4c;
  border: 1px solid #ffffff;
}
.btn-white:hover {
  color: #4c4c4c;
  background: #efefef;
  border-color: #efefef;
}

.btn-red {
  color: #de2900;
  background: transparent;
  border-color: #de2900;
}
.btn-red:hover {
  color: #ffffff;
  background: #ab3c16;
  border-color: #ab3c16;
}

.btn-green {
  background-color: #5a9359;
  border-color: #5a9359;
}
.btn-green:hover {
  background-color: #338033;
  border-color: #338033;
}

.btn-link {
  -webkit-appearance: none;
  font-size: 1rem;
  line-height: 1.5;
  cursor: pointer;
  padding: 0;
  border: none;
  color: #0079c1;
  background: transparent;
}
.btn-link:hover {
  text-decoration: underline;
}

.btn:disabled,
.btn-disabled,
button[disabled] {
  cursor: not-allowed;
  pointer-events: none;
  opacity: 0.3;
}
</style>
