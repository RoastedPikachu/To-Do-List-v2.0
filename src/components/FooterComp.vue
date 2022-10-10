<template>
  <footer>
    <a href="https://github.com/RoastedPikachu">© Сайт создан: RoastedPikachu</a>
    <p>2022</p>
  </footer>
</template>

<script>
export default {
  name: 'FooterComp',
  data(){
    return {

    }
  },
  methods: {

  }
}
</script>

<style lang="scss" scoped>
  @import url('https://fonts.googleapis.com/css2?family=Marck+Script&family=Montserrat+Alternates&display=swap');

  footer {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    margin-top: 50px;
    width: 100%;
    height: 70px;
    background: rgb(76, 99, 0);
    color: white;
    font-size: 16px;
    font-family: 'Montserrat Alternates', sans-serif;

    p {
      margin-top: 5px;
      width: 100%;
      text-align: center;
    }

    a {
      color: white;
      text-decoration: none;
      transition: 500ms ease;
    }
    a:hover {
      transform: scale(1.05);
    }
  }
</style>
