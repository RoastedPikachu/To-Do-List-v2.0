<template>
    <header class="headerDarkTheme">
      <div title="Логотип SubTrack"></div>
      <span id="headerText">
        <p id="changeThemeIcon" @click="changeTheme"><i class="fa-solid fa-circle-half-stroke"></i></p>
        <p id="entry"><a href="/" class="path">Регистрация</a>/<a href="/" class="path">Вход</a></p>
      </span>
    </header>
</template>

<script>
export default {
  name: 'HeaderComp',
  methods: {
    changeTheme() {
      document.querySelector('section').classList.toggle('lightTheme');
      document.querySelector('header').classList.toggle('headerLightTheme');
      document.querySelector('footer').classList.toggle('footerLightTheme');
      this.imagePath = require('@/assets/darkLogo.png');
    }
  }
}
</script>

<style scoped lang="scss">
  @import url('https://fonts.googleapis.com/css2?family=Marck+Script&family=Montserrat+Alternates&display=swap');

  header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 5%;
    width: 90%;  
    height: 125px;
    font-family: 'Montserrat Alternates', sans-serif;
    div {
      width: 300px;
      height: 85px;
      background-size: cover;
      transition: 500ms ease;
    }
    #headerText {
      display: flex;
      align-items: center;
      justify-content: space-between;
      width: 280px;
      #changeThemeIcon {
        font-size: 28px;
        cursor: pointer;
        transition: 500ms ease;
      }
      #changeThemeIcon:hover {
        color: #ffe100;
      }
      #entry {
        font-size: 22px;
        .path {
          text-decoration: none;
          transition: 500ms ease;
        }
        .path:hover { 
          color: #ffe100;
        }
      }
    }
  }

  .headerDarkTheme {
    background-color: #272727;
    div {
      background-image: url('@/assets/logo.png');
    }
    #headerText {
      color: #fffcf4;
      #entry {
        .path {
          color: #fffcf4;
        }
      }
    }
  }

  .headerLightTheme {
    background-color: #fffcf4;
    div {
      background-image: url('@/assets/darkLogo.png');
    }
    #headerText {
      color: #272727;
      #entry {
        .path {
          color: #272727;
        }
      }
    }
  }
</style>
