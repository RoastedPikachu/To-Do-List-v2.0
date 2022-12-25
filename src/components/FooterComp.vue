<template>
  <footer class="footerDarkTheme">
    <p>© Сайт создан: <a href="https://github.com/RoastedPikachu">RoastedPikachu</a></p>
    <p>2022</p>
  </footer>
</template>

<script>
export default {
  name: 'FooterComp',
}
</script>

<style scoped lang="scss">
  @import url('https://fonts.googleapis.com/css2?family=Marck+Script&family=Montserrat+Alternates&display=swap');

  footer {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    width: 100%;
    height: 75px;
    font-size: 16px;
    font-weight: 600;
    font-family: 'Montserrat Alternates', sans-serif;
    p {
      margin-top: 5px;
      width: 100%;
      text-align: center;
      a {
        text-decoration: none;
        transition: 500ms ease;
      }
      a:hover {
        color: #ffe100;
      }
    }
  }

  .footerDarkTheme {
    background-color: #272727;
    color: #fffcf4;
    p {
      a {
        color: #fffcf4;
      }
    }
  }

  .footerLightTheme {
    background-color: #fffcf4;
    color: #272727;
    p {
      a {
        color: #272727;
      }
    }
  }
</style>
