<template @load="al()">
  <HeaderComp/>
  <section>
    <div id="firstList">
      <h2>Список дел</h2>
      <form>
        <input @keydown="isEnter($event)" id="text" type="text" placeholder="Введите своё дело" v-model="item" required>
        <button @click="addText()" type="button">Добавить в список дел</button>
      </form>
      <ol id="list1"  @click="del($event)">
        <li class="l1" v-for="item of items" :key="item.id">
          <div>
            <p>{{ item.text }}</p>
            <span><i class="fa-solid fa-xmark"></i></span>
          </div>
        </li>
      </ol>
    </div>
    <div id="secondList">
      <h2>Список мыслей</h2>
      <form>
        <input @keydown="isEnter($event)" id="idea" type="text" placeholder="Введите свою мысль/идею" v-model="idea" required>
        <button @click="addIdea()" type="button">Добавить в список мыслей</button>
      </form>
      <ol id="list2" @click="del($event)">
        <li class="l2" v-for="idea of ideas" :key="idea.id">
          <div>
            <p>{{ idea.text }}</p>
            <span><i class="fa-solid fa-xmark"></i></span>
          </div>
        </li>
      </ol>
    </div>
  </section>
</template>

<script>
import HeaderComp from './components/HeaderComp.vue'

export default {
  name: 'App',
  data(){
    return {
      idea: '',
      item: '',
      nodeValue: '',
      startId: 0,
      itemId: 0,
      ideaId: 1,
      isEmpty: 1,
      itemEl: {},
      ideaEl: {},
      itemArr: JSON.parse(localStorage.getItem('items')) || ['Например: Сходить на пробежку'],
      ideaArr:  JSON.parse(localStorage.getItem('ideas')) || ['Например: Хорошо было бы купить беговую дорожку'],
      ideas: JSON.parse(localStorage.getItem('ideas')) || ['Например: Хорошо было бы купить беговую дорожку'],
      items: JSON.parse(localStorage.getItem('items')) || ['Например: Сходить на пробежку'],
    }
  },
  methods: {
    isEnter: function(event){
      if (event.code == 'Enter'){

        event.preventDefault();

        if (event.target.getAttribute('id') == 'text'){
          this.addText();
        } else {
          this.addIdea();
        }
      }
    },
    addText: function(){

      if (this.item != ''){

        this.itemEl.text = this.item;
        this.itemEl.id = this.itemId;

        this.itemArr.push(this.itemEl);
        localStorage.setItem('items', JSON.stringify(this.itemArr));
        this.items = JSON.parse(localStorage.getItem('items'));

        this.itemId++;  
        this.item = '';
        this.itemEl = {};
      }
    },
    addIdea: function(){
      if (this.idea != ''){

        this.ideaEl.text = this.idea;
        this.ideaEl.id = this.ideaId;

        this.ideaArr.push(this.ideaEl);
        localStorage.setItem('ideas', JSON.stringify(this.ideaArr));
        this.ideas = JSON.parse(localStorage.getItem('ideas'));
        
        this.ideaId++;
        this.idea = '';
        this.ideaEl = {};
      }
    },
    del: function(event){
      if (event.target.parentNode.tagName == 'SPAN'){

        event.target.closest('li').remove();
        this.nodeValue = event.target.parentNode.previousSibling.textContent;
        let arr = [];

        if (event.target.closest('li').className == 'l1'){

          arr = JSON.parse(localStorage.getItem('items'));
        } else if (event.target.closest('li').className == 'l2') {

          arr = JSON.parse(localStorage.getItem('ideas'));
        }

        for (let item of arr){
            if (item.text == this.nodeValue){

              arr = arr.filter(el => el.id != item.id);
              this.startId = item.id;
            
              break;
            }
        }

        if (event.target.closest('li').className == 'l1'){

          localStorage.setItem('items', JSON.stringify(arr));
          this.items = JSON.parse(localStorage.getItem('items'));
        } else if (event.target.closest('li').className == 'l2') {

          localStorage.setItem('ideas', JSON.stringify(arr));
          this.ideas = JSON.parse(localStorage.getItem('ideas'));
        }

        for (let item of arr){

          if (item.id > this.startId){

            item.id--;
            this.itemId--;
          }
        }
      }
    },
    message: function(){
      alert('При вводе элемента списка с повторяющимся текстом добавьте ему номер');
    },
    al: function(){
    },
  },
  components: {
    HeaderComp
  }
}
</script>

<style lang="scss">
  @import url('https://fonts.googleapis.com/css2?family=Marck+Script&family=Montserrat+Alternates&display=swap');

  *{
    margin:0;
    padding:0;
  }

  body {
    background-color: rgb(255, 245, 232);
  }

  section {
    display: flex;
    justify-content: space-around;
    align-items: flex-start;
    margin-top: 50px;
    width: 100%;
    height: auto;
    background-color: rgb(255, 245, 232);
    h2 {
      margin-top: 30px;
      color: rgb(76, 99, 0);
      font-size: 34px;
      font-family:  'Marck Script', cursive;
    }

    #firstList {
      display: flex;
      justify-content: center;
      align-items: center;
      flex-wrap: wrap;
      width: 35%;
      height: auto;
      background-color: white;
      box-shadow: 10px 10px 5px rgb(255, 231, 206);
      form {
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
        width: 90%;
        input {
          margin-top: 15px;
          padding-left: 10px;
          width: 90%;
          height: 40px;
          font-size: 18px;
          border: 2px solid rgb(76, 99, 0);
          border-radius: 10px;
          font-family: 'Montserrat Alternates', sans-serif;
          outline: none;
        }

        button {
          margin-top: 30px;
          width: 55%;
          height: 40px;
          background-color: rgb(255, 222, 139);
          border: 0;
          border-radius: 10px;
          font-size: 16px;
          font-family: 'Montserrat Alternates', sans-serif;
          cursor: pointer;
          transition: 500ms ease;
        }
        button:hover {
          color: white;
          background-color: rgb(255, 207, 85);
          box-shadow: 5px 5px 5px rgb(255, 239, 197);
        }
        button:active {
          transform: translate(1px, 3px);
        }
      }

      #list1 {
        display: flex;
        justify-content: flex-start;
        flex-wrap: wrap;
        margin-top: 20px;
        margin-left: 65px;
        padding-bottom: 40px;
        width: 90%;
        font-size: 22px;
        font-family: 'Marck Script', cursive;
        li {
          width: 100%;
          div {
            display: flex;
            justify-content: flex-start;
            flex-wrap: wrap;
            padding-top: 10px;
            padding-left: 10px;
            width: 90%;
            height: auto;
            border: 0 solid rgb(76, 99, 0);
            border-width: 0 0 1.5px 0;
            color: rgb(76, 99, 0);
            font-family: 'Marck Script', cursive;
            p {
              width: 95%;
              font-size: 22px;
            }

            span {
              color: black;
              font-size: 20px;
              cursor: pointer;
            }
          } 
        }
      }
    }

    #secondList {
      display: flex;
      justify-content: center;
      align-items: center;
      flex-wrap: wrap;
      width: 35%;
      height: auto;
      background-color: white;
      box-shadow: 10px 10px 5px rgb(255, 231, 206);
      h2 {
        margin-top: 30px;
        color: rgb(76, 99, 0);
        font-size: 34px;
        font-family:  'Marck Script', cursive;
      }

      form {
          display: flex;
          justify-content: center;
          flex-wrap: wrap;
          width: 90%;
          input {
            margin-top: 15px;
            padding-left: 10px;
            width: 90%;
            height: 40px;
            font-size: 18px;
            border: 2px solid rgb(76, 99, 0);
            border-radius: 10px;
            font-family: 'Montserrat Alternates', sans-serif;
            outline: none;
          }

          button {
            margin-top: 30px;
            width: 60%;
            height: 40px;
            background-color: rgb(255, 222, 139);
            border: 0;
            border-radius: 10px;
            font-size: 16px;
            font-family: 'Montserrat Alternates', sans-serif;
            cursor: pointer;
            transition: 500ms ease;
          }
          button:hover {
            color: white;
            background-color: rgb(255, 207, 85);
            box-shadow: 5px 5px 5px rgb(255, 239, 197);
          }
          button:active {
            transform: translate(1px, 3px);
          }
        }
      }

      #list2 {
        display: flex;
        justify-content: flex-start;
        flex-wrap: wrap;
        margin-top: 20px;
        margin-left: 65px;
        padding-bottom: 40px;
        width: 90%;
        font-size: 22px;
        font-family: 'Marck Script', cursive;
        li {
          width: 100%;
          div {
            display: flex;
            justify-content: flex-start;
            flex-wrap: wrap;
            padding-top: 10px;
            padding-left: 10px;
            width: 90%;
            height: auto;
            border: 0 solid rgb(76, 99, 0);
            border-width: 0 0 1.5px 0;
            color: rgb(76, 99, 0);
            font-family: 'Marck Script', cursive;
            p {
              width: 90%;
              font-size: 22px;
            }
          
            span {
              margin-left: 5%;
              color: black;
              font-size: 20px;
              cursor: pointer;
            }
          }
        }
      }
    }
</style>
