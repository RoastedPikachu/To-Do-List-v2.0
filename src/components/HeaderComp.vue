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
    background: rgb(67, 67, 67);
    color: white;
    font-size: 22px;
    font-family: 'Montserrat Alternates', sans-serif;
  }
</style>
