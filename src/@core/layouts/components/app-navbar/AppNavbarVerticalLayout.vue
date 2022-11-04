<template>
  <div class="navbar-container d-flex content align-items-center">
    <b-navbar-nav class="nav align-items-center ml-auto">
      <dark-Toggler class="d-none d-lg-block" />

      <user-dropdown />
    </b-navbar-nav>
  </div>
</template>

<script>
import { BLink, BNavbarNav } from "bootstrap-vue";

import DarkToggler from "./components/DarkToggler.vue";

import UserDropdown from "./components/UserDropdown.vue";

export default {
  components: {
    BLink,
    // Navbar Components
    BNavbarNav,
    DarkToggler,
    UserDropdown,
  },
  props: {
    toggleVerticalMenuActive: {
      type: Function,
      default: () => {},
    },
  },
};
</script>
