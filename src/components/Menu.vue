<template>
  <nav class="nav-extended">
    <div class="nav-wrapper">
      <a href="#!" class="brand-logo">LogoInventarioPoner</a>
      <ul class="right hide-on-med-and-down">
        <li>
          <router-link to="/">INICIO</router-link>
        </li>
        <li>
          <router-link to="/area">AREAS</router-link>
        </li>
        <li>
          <router-link to="/activos">ACTIVOS</router-link>
        </li>
      </ul>
    </div>
    <div class="nav-content">
      <span class="nav-title">Inventario</span>
    </div>
  </nav>
</template>

<script>
export default {
  name: "MenuComponents",
  props: {
    msg: String,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss"></style>
