<template>
  <div class="app-container">
    <TopNav :cartItemCount="cartItemCount"/>
    
    <main class="main-content">
      <router-view
        :products="products"
        :cartItems="cartItems"
        @addToCart="addToCart"
        @removeFromCart="removeFromCart"
        @submitOrder="submitOrder"
      ></router-view>
    </main>

    <footer class="site-footer">
      <p>&copy; 2025 Best Buy Clone - Cloud Native Project</p>
    </footer>
  </div>
</template>

<script>
import TopNav from './components/TopNav.vue'

export default {
  name: 'App',
  components: {
    TopNav
  },
  data() {
    return {
      cartItems: [],
      products: [],
    }
  },
  computed: {
    cartItemCount() {
      return this.cartItems.reduce((total, item) => {
        return total + item.quantity
      }, 0)
    }
  },
  mounted() {
    this.getProducts()
  },
  methods: {
    getProducts() {
      // NOTE: This fetch works with the Rust Product-Service we just discussed
      fetch('/products') 
        .then(response => response.json())
        .then(products => {
          console.log('Product catalog loaded successfully')
          this.products = products
        })
        .catch(error => {
          console.error(error)
          // In a real app, use a Toast notification instead of alert
          console.log('Could not load products. Is the Product-Service running?')
        })
    },
    addToCart({ productId, quantity }) {
      const existingCartItem = this.cartItems.find(
        item => item.product.id == productId
      )
      if (existingCartItem) {
        existingCartItem.quantity += quantity
      } else {
        const product = this.products.find(product => product.id == productId)
        this.cartItems.push({ product, quantity })
      }
    },
    removeFromCart(index) {
      this.cartItems.splice(index, 1)
    },
    submitOrder() {
      const order = {
        customerId: "User-" + Math.floor(Math.random() * 10000), // Mock ID
        items: this.cartItems.map(item => {
          return {
            productId: item.product.id,
            quantity: item.quantity,
            price: item.product.price
          }
        })
      }

      console.log("Submitting Order:", JSON.stringify(order));

      fetch(`/order`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(order)
      })
        .then(response => {
          if (!response.ok) {
            alert('Error: Could not place order.')
          } else {
            this.cartItems = []
            alert('Order placed successfully! Thank you for shopping at Best Buy.')
          }
        })
        .catch(error => {
          console.log(error)
          alert('Network Error: Order service unavailable.')
        })
    }
  },
}
</script>

<style>
/* --- GLOBAL RESET & BEST BUY THEME --- */

body {
  /* Removed 'algonquin.jpg' */
  background-color: #f4f6f8; /* Light Gray Background */
  margin: 0;
  padding: 0;
  min-height: 100vh;
}

#app {
  font-family: 'Human BBY', Helvetica, Arial, sans-serif; /* Clean corporate font */
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #1d252c; /* Dark Grey text, not pure black */
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

/* --- LAYOUT --- */
.main-content {
  /* Push content down so it's not hidden behind fixed header */
  /* Ensure footer stays at bottom */
  flex: 1;
  padding-top: 80px; 
  padding-bottom: 60px;
  max-width: 1200px;
  margin: 0 auto;
  width: 100%;
}

/* --- HEADER & FOOTER (Best Buy Blue) --- */
nav, .site-footer {
  background-color: #0046be; /* Official Best Buy Blue */
  color: #fff;
}

.site-footer {
  text-align: center;
  padding: 1rem;
  font-size: 0.9rem;
}

/* --- BUTTONS (Best Buy Yellow) --- */
button {
  padding: 0 20px;
  background-color: #ffe000; /* Best Buy Yellow */
  color: #001e73; /* Dark Blue text for contrast */
  border: none;
  border-radius: 4px; /* Slightly sharper corners */
  cursor: pointer;
  height: 40px;
  font-weight: bold;
  font-size: 14px;
  transition: background-color 0.2s;
}

button:hover {
  background-color: #fff200; /* Lighter yellow on hover */
}

/* --- PRODUCT CARDS --- */
.product-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 20px;
  padding: 20px;
}

.product-card {
  background-color: #fff;
  border: 1px solid #e0e6ef;
  border-radius: 8px;
  padding: 1.5rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  transition: box-shadow 0.2s;
}

.product-card:hover {
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}

.product-card img {
  max-height: 200px; /* Limit image height */
  object-fit: contain;
  margin-bottom: 1rem;
}

.product-card h2 {
  font-size: 1.1rem;
  line-height: 1.4;
  margin: 0.5rem 0;
  color: #0046be; /* Blue title */
}

.product-price {
  font-size: 1.5rem;
  font-weight: 700;
  margin: 10px 0;
}

/* --- CART TABLE --- */
.shopping-cart {
  background-color: #fff;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  margin-top: 20px;
}

.shopping-cart-table th {
  border-bottom: 2px solid #0046be;
  color: #0046be;
}
</style>