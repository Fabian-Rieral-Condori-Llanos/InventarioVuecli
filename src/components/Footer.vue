<template>
  <footer class="page-footer">
    <div class="container">
      <div class="row">
        <div class="col l6 s12">
          <h5 class="white-text">Inventrio De prueba</h5>
          <p class="grey-text text-lighten-4">
            INventrios para ir registrando modelo de prueba con materialize
          </p>
        </div>
        <div class="col l4 offset-l2 s12">
          <ul>
            <li><a class="grey-text text-lighten-3" href="#!"></a></li>
            <li><a class="grey-text text-lighten-3" href="#!">GitHub</a></li>
            <li><a class="grey-text text-lighten-3" href="#!">GitLab</a></li>
            <li><a class="grey-text text-lighten-3" href="#!">Linkedln</a></li>
          </ul>
        </div>
      </div>
    </div>
    <div class="footer-copyright">
      <div class="container">© 2023 Copyright</div>
    </div>
  </footer>
</template>

<script>
export default {
  name: "FooterComponents",
  props: {
    msg: String,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss"></style>
