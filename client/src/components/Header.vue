<template>
    <div>
        <header>
            <img v-if="!opened" @click="burgerAction" class="burger" src="../assets/images/burger.png">
            <img v-else="opened" @click="burgerAction" class="burger" src="../assets/images/burger-close.png">
            <img src="../assets/images/odezenne_logotype.svg" :class="{goWhite: opened}" @click="burgerAction" id="logo_header">
            <div class="panier">
                <span>({{tickets.length}})</span>
                <img v-if="tickets.length" @click="openCartAction" src="../assets/images/panier_1.svg" alt="panier">
                <img v-else="!tickets.length" @click="openCartAction" src="../assets/images/panier_vide.svg"alt="panier">
                <cart :isOpen="isCartOpened"></cart>
            </div>
        </header>
        <nav :class="{active: opened}">
            <router-link class="router-link" :to="{ name: 'Home' }" exact @click.native="burgerAction">Giga Timeline</router-link>
            <a href="#" class="router-link">Shop</a>
            <router-link class="router-link" :to="{ name: 'Billetterie' }" @click.native="burgerAction">Billetterie</router-link>
            <a href="#" class="router-link">Historique</a>
            <router-link class="router-link" :to="{ name: 'Blog' }" @click.native="burgerAction">Blog</router-link>
            <div class="RS">
                <a href="https://twitter.com/odezenne" target="_blank"><img src="../assets/images/twitter.png"></a>
                <a href="https://www.facebook.com/odezenne/" target="_blank"><img src="../assets/images/facebook-logo.png"></a>
                <a href="https://www.instagram.com/odezenne_/?hl=fr" target="_blank"><img src="../assets/images/instagram.png"></a>
                <a href="https://www.youtube.com/user/alo2zen" target="_blank"><img src="../assets/images/youtube.png"></a>
                <a href="https://soundcloud.com/odezenne" target="_blank"><img src="../assets/images/soundcloud.png"></a>
            </div>
        </nav>
        <div v-if="opened" :class="{active: opened, overlay: true}"></div>
    </div>
</template>

<script>
  import { mapGetters } from 'vuex';

  import cart from './Ticket/Cart';

  export default {
    name: 'OHeader',
    components: {
      cart,
    },
    data() {
      return {
        opened: false,
        isCartOpened: false,
      };
    },
    mounted() {
      document.onclick = (e) => {
        if (e.target.classList.contains('overlay')) {
          this.opened = false;
        }
      };
    },
    computed: {
      ...mapGetters({
        tickets: 'cartTickets',
      }),
    },
    methods: {
      burgerAction() {
        this.opened = !this.opened;
      },
      openCartAction() {
        this.isCartOpened = !this.isCartOpened;
      },
    },
  };
</script>

<style lang="scss" scoped>
    header {
        display: flex;
        justify-content: space-between;
        align-items: flex-start;
        position: relative;
        height: 200px;
        padding: 30px;
        z-index: 12;
    }
    nav {
        transition: all 0.6s ease;
        display: flex;
        flex-direction: column;
        justify-content: flex-end;
        align-items: center;
        background-color: rgba(0,0,0,0.9);
        position: absolute;
        top: -550px;
        width: 100%;
        height: 550px;
        z-index: 10;
    }
    nav>a {
        color: #fff;
        text-decoration: none;
        padding: 17px 0;
        text-transform: uppercase;
    }
    nav a {
        opacity: 0;
    }
    nav.active {
        top: 0;
    }
    nav.active a {
        animation-name: nav-appear;
        animation-duration: 500ms;
        animation-delay: 250ms;
        animation-fill-mode: forwards;
    }
    header>div {
        transition: all 0.6s ease;
    }
    .goWhite {
        color: #fff;
    }
    .RS {
        display: flex;
        justify-content: center;
        padding: 30px 0px 12px 0px;
    }
    .RS img {
        width: 20px;
    }
    .RS>a {
        color: #fff;
        text-decoration: none;
        padding: 10px 15px;
    }
    .RS>a:hover {
        cursor: pointer;
    }
    .panier img{
        width:20px;
    }
    #logo_header, .panier img {
        transition: all 0.6s ease;
        height: 100%;
    }
    .router-link {
        position: relative;
    }
    .router-link::before {
        transition: all 0.4s ease;
        display: block;
        content: '';
        height: 1px;
        position: absolute;
        top:0;
        bottom:2px;
        margin: auto;
        width:0;
        background-color: #fff;
    }
    .router-link.router-link-active::before, .router-link:hover::before {
        width:100%;
    }
    .burger {
        width: 24px;
    }
    .panier {
        display: flex;
        align-items: center;
    }
    .panier span {
        padding-right: 8px;
    }
    .burger:hover, .panier:hover {
        cursor: pointer;
    }
    .panier svg {
        width: 20px;
    }
    .panier.goWhite svg {
        fill: #fff;
    }
    .overlay {
      position:fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: #000;
      opacity: 0;
      transition: all 0.6s ease;
      &.active{
        z-index: 9;
      }
    }
    @keyframes nav-appear {
        0% { opacity: 0 }
        100% { opacity: 1 }
    }
</style>
