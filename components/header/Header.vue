<template>
  <nav
    class="flex justify-between sticky top-0 bg-gray_900 z-10 px-6 py-4"
    role="navigation"
    aria-label="main navigation"
  >
    <nuxt-link :to="{ Nombre: 'index' }" class="navbar-item">
      <h1 class="title w-48 h-12"></h1>
    </nuxt-link>
    <div>
    <div class="flex items-center">
      <div class="mx-4">
        <div class="cursor-pointer pt-4" @click="showCheckoutModal">
          <span :class="[numProductsAdded > 0 ? 'p-2 bg-blue text-white rounded-md' : 'text-grey_light']">{{ numProductsAdded }}</span>
          <span class="icon text-grey_light">
            <i class="fa fa-shopping-cart"></i>
          </span>
        </div>
      </div>

      <div class="mx-4 pt-4">
        <button v-if="!isUserLoggedIn" @click="onShowDropdown">
          <span class="icon text-grey_light">
            <i class="fa fa-user"></i>
          </span>
        </button>
        <button class="cursor-pointer" v-if="isUserLoggedIn" @click="onShowDropdown">
          Bienvenido {{ getUserName }}
        </button>
        <div v-if="showDropdown && isUserLoggedIn" class="dropdown w-52 h-28">
          <nuxt-link :to="{ name: 'user-wishlist' }" class="button text-center">
            <span class="text-lg">{{ wishlistLabel }}</span>
          </nuxt-link>
          <!-- <button @click="logout" class="button">
            <span class="text-lg">{{ logoutLabel }}</span>
          </button> -->
        </div>
        <div v-if="showDropdown && !isUserLoggedIn" class="dropdown">
          <button v-if="!isUserLoggedIn" class="button" @click="showLoginModal">
            <span class="text-lg">¿Ya está registrado?<br /> {{ loginLabel }}</span>
            <i class="fa fa-sign-in"></i>
          </button>
          <button v-if="!isUserLoggedIn" class="button" @click="showSignupModal">
            <span class="text-lg">¿Nuevo Usuario?<br /> {{ signupLabel }}</span>
            <i class="fa fa-user-plus"></i>
          </button>
        </div>
      </div>
    </div>
    </div>
  </nav>
</template>

<script>
  export default {
    name: 'VmHeader',

    data () {
      return {
        isCheckoutActive: false,
        showDropdown: false,
        logoutLabel: 'Cerrar sesión',
			  loginLabel: 'Iniciar sesión',
			  signupLabel: 'Regístrate',
        wishlistLabel: 'Favoritos',
      }
    },

    computed: {
      numProductsAdded () {
        return this.$store.getters.productsAdded.length;
      },
      isUserLoggedIn () {
        return this.$store.getters.isUserLoggedIn;
      },
      getUserName () {
        let name = this.$store.getters.getUserName;

        if (name === '') {
          return 'Usuario';
        } else {
          return name;
        }
      }
    },

    mounted() {
      window.addEventListener("blur", this.closeDropdown, true);
    },
    destroyed() {
      window.removeEventListener("blur", this.closeDropdown);
    },

    methods: {
      closeDropdown() {
        setTimeout(() => {
          this.showDropdown = false;
        }, 100);
      },
      showCheckoutModal () {
        this.$store.commit('showCheckoutModal', true);
      },
      showLoginModal () {
        this.$store.commit('showLoginModal', true);
      },
      showSignupModal () {
        this.$store.commit('showSignupModal', true);
      },
      onShowDropdown () {
        this.showDropdown = !this.showDropdown
      },
      // logout () {
      //   this.$store.commit('isUserLoggedIn', false);
      //   this.$store.commit('isUserSignedUp', false);
      //   this.$store.commit('removeProductsFromFavourite');

      //   // redirect to homepage
      //   this.$router.push({ name: 'index' });
      // },
    }
  };
</script>

<style lang="scss" scoped>
  .title {
    background: url('../../static/azael.png') no-repeat;
    background-position: 50% 50%;
    background-size: 165px;
  }
  .dropdown {
    @apply absolute;
    @apply p-3;
    @apply bg-white;
    @apply right-0;
    @apply shadow-lg;
    @apply rounded-xl;
    @apply flex;
    @apply flex-col;
    @apply border-2;
    @apply border-grey_light;
  }

  .button {
    @apply w-full;
    @apply hover:bg-grey_light;
    @apply p-2;
    @apply rounded-lg;
  }
</style>
