<template>
  <div id="app">
    <header>
        <div class="logo">E-shop</div>
        <div class="cart">
            <button class="btn-cart" type="button" @click="$refs.cart.cartVisible = !$refs.cart.cartVisible">Корзина</button>
            <cart ref="cart"></cart>
        </div>
    </header>
    <main>
        <catalog></catalog>
    </main>
  </div>
</template>

<script>
import catalog from './components/Catalog'
import cart from './components/Cart'

export default {
  name: 'App',
  methods: {
      getJSON(url) {
          return fetch(url)
              .then(result => result.json())
              .catch(err => {
                  console.error(err)
              })
      },
      postJSON(url, data) {
          return fetch(url, {
              method: 'POST',
              headers: {
                  "Content-Type": "application/json"
              },
              body: JSON.stringify(data)
          })
              .then(result => result.json())
              .catch(err => {
                  console.error(err)
              })
      },
      putJSON(url, data) {
          return fetch(url, {
              method: 'PUT',
              headers: {
                  "Content-Type": "application/json"
              },
              body: JSON.stringify(data)
          })
              .then(result => result.json())
              .catch(err => {
                  console.error(err)
              })
      },
      deleteJSON(url, data) {
          return fetch(url, {
              method: 'DELETE',
              headers: {
                  "Content-Type": "application/json"
              },
              body: JSON.stringify(data)
          })
              .then(result => result.json())
              .catch(err => {
                  console.error(err)
              })
      }
  },
  mounted() {},
  components: {
      catalog,
      cart
  }
}
</script>