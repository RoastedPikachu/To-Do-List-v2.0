<template>
    <header>
      <h1>Список для ваших задач и мыслей</h1>
    </header>
</template>

<script>
export default {
  name: 'HeaderComp',
  data(){
    return {
    }
  },
  methods: {
    
  }
}
</script>

<style scoped>
  @import url('https://fonts.googleapis.com/css2?family=Marck+Script&family=Montserrat+Alternates&display=swap');

  header {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100px;
    background: rgb(76, 99, 0);
    color: white;
    font-size: 22px;
    font-family: 'Montserrat Alternates', sans-serif;
  }
  @media (max-width: 425px){
    header {
      font-size: 14px;
      text-align: center;
    }
  }
</style>
