<template>
  <header>
    <nav v-scroll="updateScroll" class="nav p-1" id="drawer"
      :class="{ 'bg-primary': $route.path !== '/' || scrollPosition > 20}">
      <div class="brand">
        <p class="brand-name p-1">Meathycal</p>
      </div>
      <ul class="menu-desktop">
        <li class="nav-item mr-5">
          <router-link :to="'/'">Home</router-link>
        </li>
        <li class="nav-item mr-5">
          <router-link :to="'menu'">Menu</router-link>
        </li>
        <li class="nav-item mr-5">
          <router-link :to="'gallery'">Gallery</router-link>
        </li>
        <li class="nav-item mr-5" id="dropdown">
          <router-link :to="'cart'">Cart</router-link>
          <span class="badge" v-if="cart.length > 0"> {{cartItemCount}}</span>
          <div class="dropdown-content text-primary p-2">
            <div v-if="cart.length === 0">
              <p class="text-center">Oops.. Your Cart Still Empty</p>
            </div>
            <div v-else>
              <review-item v-for="item in cart" :key="item.product.id" :cartItem="item"></review-item>
              <div class="cart-item-footer flex justify-between pt-2 mt-2 ">
                <buttton-primary @button-click="clearCartItems" :buttonText="'Clear Cart'" />
                <router-link to="cart">
                  <buttton-primary :buttonText="'Check Cart'" />
                </router-link>
              </div>
            </div>
          </div>
        </li>
      </ul>

      <a @click="toggle" id="hamburger" class="p-1" aria-label="Hamburger">☰</a>
    </nav>
          <ul ref="mobileMenu" class="menu-mobile p-3 bg-primary text-white">
              <li class="nav-item ">
          <router-link :to="'/'">Home</router-link>
        </li>
        <li class="nav-item ">
          <router-link :to="'menu'">Menu</router-link>
        </li>
        <li class="nav-item ">
          <router-link :to="'gallery'">Gallery</router-link>
        </li>
        <li class="nav-item" id="dropdown">
          <router-link :to="'cart'">Cart
              <span class="badge" v-if="cart.length > 0"> {{cartItemCount}}</span>
          </router-link>
        
        </li>
      </ul>
  </header>
</template>

<script>
import ReviewItem from './ReviewItem.vue';
import {mapGetters, mapState, mapActions} from 'vuex';
import ButttonPrimary from './ButttonPrimary.vue';
export default {
  name: 'Navbar',
  components:{ ReviewItem, ButttonPrimary },
  data(){
   return{
      scrollPosition: null,  
    }
  },
  computed :{
    ...mapGetters({
      cartItemCount :"cartItemCount"
    }),
    ...mapState([
      'cart'
    ]),
  },
  methods: {
    ...mapActions(["clearCartItems"]),
     updateScroll() {
       this.scrollPosition = window.scrollY
    },
    toggle(){
      this.$refs.mobileMenu.classList.toggle('translate-normal');

    }
  },
  mounted(){
        window.addEventListener('scroll', this.updateScroll);
  },
};
</script>
<style lang="scss" >
@import "./../scss/_responsive.scss";
// MOBILE NAV
.menu-mobile{
  position: fixed;
  top: 0;
  list-style: none;
  width: 100%;
  transform: translate(0, -200px);
  transition: all 0.3s ease-in-out;
  z-index: 2;
   border-bottom-left-radius: 25px;
  border-bottom-right-radius: 25px;
  @include tablet {
    display: none;
  }
  li{
    a{
          position: relative;

      color: white;
      display: inline-block;
    }
    .badge {
        display: flex;
        align-items: center;
        justify-content: center;
        position: absolute;
       top: 0;
       right: -20px;
        font-size: 12px;

        width: 20px;
        height: 20px;
        border-radius: 50%;
        background-color: #f9f9f9;
        color: #2B2B2B;
      }
  }
}
.translate-normal{
  transform: translate(0, 50px);
}
// DEFAULT / DESKTOP NAV
nav {
  color: white;
  display: flex;
  width: 100%;
  top: 0;
  position: fixed;
  background-color: #2B2B2B;
  justify-content: space-between;
  align-items: center;
  transition: background-color 1s ease;
  z-index: 10;
  border-bottom-left-radius: 25px;
  border-bottom-right-radius: 25px;

  @include tablet {
    background-color: transparent;
  }

  .brand-name {
    font-weight: bold;
    font-size: 21px;
  }

  ul {
    &.menu-desktop{
          display: none;
    }
    margin-left: auto;
    list-style: none;
    flex-direction: column;

    @include tablet {
      flex-direction: row;
      &.menu-desktop{
          display: flex;
    }
     
    }

    li {

      a {
        color: white;
        font-weight: 600;
      }

      .badge {
        display: flex;
        align-items: center;
        justify-content: center;
        position: absolute;
        top: 15px;
        font-size: 12px;
        right: 25px;
        width: 20px;
        height: 20px;
        border-radius: 50%;
        background-color: #f9f9f9;
        color: #2B2B2B;
      }

      &#dropdown:hover {
        .dropdown-content {
          visibility: visible;
          opacity: 1;
          transform: translate(0, 45px);
          transition: all 0.5s ease-in-out;
        }
      }

      .dropdown-content {
        visibility: hidden;
        position: absolute;
        right: 10px;
        background-color: #f9f9f9;
        min-width: 350px;
        top: 0;
        box-shadow: 0px 8px 16px 0px rgba(0, 0, 0, 0.2);
        z-index: 1;
        opacity: 0;
        transition: all 0.5s ease-in-out;

        .cart-item-footer {
          border-top: 0.5px solid black;
        }

        button {
          border: 0;
          color: white;
          border-radius: 5px;
        }
      }
    }
  }

  #hamburger {
    font-size: 30px;
    @include tablet {
      display: none;
    }
  }
}

</style>

