<template>
  <nav class="navbar">
    <div class="logo-container">
      <router-link to="/" class="brand-logo">
        <span class="yellow-tag"></span>
        <span class="brand-text">Best Buy</span>
      </router-link>
    </div>

    <div class="search-bar">
      <input type="text" placeholder="Search for laptops, tvs, and more..." />
      <button class="search-btn">
        🔍
      </button>
    </div>

    <button class="hamburger" @click="toggleNav">
      <span class="hamburger-icon"></span>
    </button>

    <ul class="nav-links" :class="{ 'nav-links--open': isNavOpen }">
      <li><router-link to="/" @click="closeNav">Deals</router-link></li>
      <li><router-link to="/" @click="closeNav">Support</router-link></li>
      
      <li class="cart-link">
        <router-link to="/cart" @click="closeNav" class="cart-container">
          <span class="cart-icon">🛒</span>
          <span class="cart-text">Cart</span>
          <span v-if="cartItemCount > 0" class="cart-badge">{{ cartItemCount }}</span>
        </router-link>
      </li>
    </ul>
  </nav>
</template>

<script>
export default {
  name: 'TopNav',
  props: ['cartItemCount'],
  data() {
    return {
      isNavOpen: false
    }
  },
  methods: {
    toggleNav() {
      this.isNavOpen = !this.isNavOpen
    },
    closeNav() {
      this.isNavOpen = false
    }
  }
}
</script>

<style scoped>
/* --- CONTAINER --- */
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #0046be; /* Best Buy Blue */
  color: #fff;
  padding: 15px 5%;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  height: 50px; /* Fixed height for consistency */
}

/* --- LOGO (CSS imitation of Best Buy Logo) --- */
.brand-logo {
  text-decoration: none;
  color: #fff;
  font-size: 1.5rem;
  font-weight: 900;
  position: relative;
  display: flex;
  align-items: baseline;
}

.yellow-tag {
  background-color: #ffe000; /* Best Buy Yellow */
  width: 40px;
  height: 25px;
  display: inline-block;
  position: absolute;
  bottom: 0;
  left: -10px;
  z-index: -1;
  transform: skew(-10deg); /* The classic tag shape */
}

/* --- SEARCH BAR --- */
.search-bar {
  flex: 1;
  max-width: 600px;
  margin: 0 2rem;
  display: flex;
}

.search-bar input {
  width: 100%;
  padding: 8px 12px;
  border: none;
  border-radius: 4px 0 0 4px;
  font-size: 14px;
}

.search-btn {
  background-color: #fff;
  border: none;
  border-left: 1px solid #ccc;
  border-radius: 0 4px 4px 0;
  cursor: pointer;
  padding: 0 10px;
}

/* --- LINKS --- */
.nav-links {
  display: flex;
  list-style: none;
  align-items: center;
  margin: 0;
  padding: 0;
  font-weight: 600;
  font-size: 0.9rem;
}

.nav-links li {
  margin-left: 20px;
}

.nav-links a {
  color: #fff;
  text-decoration: none;
  transition: color 0.2s;
}

.nav-links a:hover {
  color: #ffe000; /* Yellow on hover */
}

/* --- CART --- */
.cart-container {
  display: flex;
  align-items: center;
  position: relative;
}

.cart-icon {
  font-size: 1.2rem;
  margin-right: 5px;
}

.cart-badge {
  background-color: #ffe000;
  color: #000;
  font-size: 0.75rem;
  font-weight: bold;
  padding: 2px 6px;
  border-radius: 50%;
  position: absolute;
  top: -8px;
  right: -10px;
}

/* --- HAMBURGER (Mobile) --- */
.hamburger {
  display: none;
  background: none;
  border: none;
  cursor: pointer;
}

.hamburger-icon {
  display: block;
  width: 25px;
  height: 3px;
  background-color: #fff;
  position: relative;
}

.hamburger-icon::before,
.hamburger-icon::after {
  content: '';
  width: 25px;
  height: 3px;
  background-color: #fff;
  position: absolute;
  left: 0;
}
.hamburger-icon::before { top: -8px; }
.hamburger-icon::after { top: 8px; }

/* --- MOBILE RESPONSIVE --- */
@media (max-width: 768px) {
  .search-bar {
    display: none; /* Hide search on small screens to save space */
  }

  .nav-links {
    display: none;
    position: absolute;
    top: 60px; /* Below navbar */
    left: 0;
    right: 0;
    background-color: #003796; /* Slightly darker blue */
    flex-direction: column;
    padding: 1rem;
    align-items: flex-start;
  }

  .nav-links--open {
    display: flex;
  }

  .nav-links li {
    margin: 10px 0;
    width: 100%;
  }

  .hamburger {
    display: block;
  }
}
</style>