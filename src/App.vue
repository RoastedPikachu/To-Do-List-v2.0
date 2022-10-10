<template>
  <HeaderComp/>
  <section>
    <div id="firstList">
      <h2>Список дел:</h2>
      <form>
        <input @keydown="isEnter($event)" id="text" type="text" placeholder="Введите своё дело" v-model="item" required>
        <button @click="addText()" type="button">Добавить в список дел</button>
      </form>
      <ol id="list1"  @click="whatAct($event)">
        <li class="l1" v-for="item of items" :key="item.id">
          <div>
            <p>{{ item.text }}</p>
            <span>
              <i class="fa-solid fa-pencil"></i>
              <i class="fa-solid fa-xmark del"></i>
            </span>
          </div>
        </li>
      </ol>
    </div>
    <div id="secondList">
      <h2>Список мыслей:</h2>
      <form>
        <input @keydown="isEnter($event)" id="idea" type="text" placeholder="Введите свою мысль/идею" v-model="idea" required>
        <button @click="addIdea()" type="button">Добавить в список мыслей</button>
      </form>
      <ol id="list2" @click="whatAct($event)">
        <li class="l2" v-for="idea of ideas" :key="idea.id">
          <div>
            <p>{{ idea.text }}</p>
            <span>
              <i class="fa-solid fa-pencil"></i>
              <i class="fa-solid fa-xmark del"></i>
            </span>
          </div>
        </li>
      </ol>
    </div>
  </section>
  <FooterComp/>
</template>

<script>
import HeaderComp from './components/HeaderComp.vue';
import FooterComp from './components/FooterComp.vue';

export default {
  name: 'App',
  data(){
    return {
      idea: '',
      item: '',
      nodeValue: '',
      prevText: '',
      count: 0,
      startId: 0,
      itemId: JSON.parse(localStorage.getItem('itemId')),
      ideaId: JSON.parse(localStorage.getItem('ideaId')),
      index: 0,
      itemEl: {},
      ideaEl: {},
      itemArr:  JSON.parse(localStorage.getItem('items')) || [],
      ideaArr:  JSON.parse(localStorage.getItem('ideas')) || [],
      ideas: JSON.parse(localStorage.getItem('ideas')) || [],
      items: JSON.parse(localStorage.getItem('items')) || [], 
    }
  },
  methods: {
    isEnter: function(event){
      if (event.code == 'Enter'){

        event.preventDefault();

        if (event.target.tagName == 'TEXTAREA'){

          this.edited(event, event.target.value);

        } 

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
        localStorage.setItem('itemId', JSON.stringify(this.itemId));
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
        localStorage.setItem('ideaId', JSON.stringify(this.ideaId));
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

            this.startId = item.id;
            arr = arr.filter(el => el.id != item.id);
            
          } else if(item.id > this.startId) {
          
            item.id--;

          }
        }

        if (event.target.closest('li').className == 'l1'){

          this.itemId--;      
          this.itemArr = arr;
          this.items = arr

          localStorage.setItem('items', JSON.stringify(arr));
          localStorage.setItem('itemId', JSON.stringify(this.itemId));

        } else if (event.target.closest('li').className == 'l2') {

          this.ideaId--;     
          this.ideaArr = arr;
          this.ideas = arr;

          localStorage.setItem('ideas', JSON.stringify(arr));
          localStorage.setItem('ideaId', JSON.stringify(this.ideaId));


        }

      }
    },
    whatAct: function(event){
      if (event.target.classList.contains('del')){

        this.del(event);

      } else {

        this.edit(event);

      }
    },
    edit: function(event){
      if (event.target.parentNode.tagName == 'SPAN' && this.count < 1){

        let parentNode = event.target.closest('div');

        let editInput = document.createElement('textarea');
        editInput.value = event.target.parentNode.previousSibling.textContent;
        this.prevText = editInput.value;

        parentNode.replaceChild(editInput, parentNode.children[0]);
        
        parentNode.children[0].addEventListener('keydown', this.isEnter);
        this.count++;

      }
    },
    edited: function(event, value){

      this.nodeValue = value;

      let arr = [];

      if (event.target.closest('li').className == 'l1'){

        arr = JSON.parse(localStorage.getItem('items'));

      } else if (event.target.closest('li').className == 'l2') {

        arr = JSON.parse(localStorage.getItem('ideas'));

      }

      for (let item of arr){
        if (item.text == this.prevText){

          let valueObj = {text: value, id: item.id}
            
          arr.splice(item.id, 1, valueObj);

          break;

        }
      }

      if (event.target.closest('li').className == 'l1'){

        this.itemArr = arr;
        localStorage.setItem('items', JSON.stringify(arr));
        this.items = JSON.parse(localStorage.getItem('items'));

      } else if (event.target.closest('li').className == 'l2') {

        this.ideaArr = arr;
        localStorage.setItem('ideas', JSON.stringify(arr));
        this.ideas = JSON.parse(localStorage.getItem('ideas'));

      }

      let parentNode = event.target.closest('div');

      let blockP = document.createElement('p');
      blockP.innerHTML = this.nodeValue;

      parentNode.replaceChild(blockP, parentNode.children[0]);

      this.count = 0;

    }
  },
  components: {
    HeaderComp,
    FooterComp
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
    min-height: 600px;
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
              width: 87.5%;
              font-size: 22px;
              word-wrap: break-word;
            }

            textarea {
              width: 87.5%;
              border: 0;
              color: rgb(76, 99, 0);
              font-size: 22px;
              font-family: 'Marck Script', cursive;
              outline: none;
              resize: none;
            }

            span {
              display: flex;
              justify-content: space-between;
              width: 12.5%;
              color: black;
              font-size: 20px;
              i {
                cursor: pointer;
              }
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
              width: 87.5%;
              font-size: 22px;
              word-wrap: break-word;
            }

            textarea {
              width: 87.5%;
              border: 0;
              color: rgb(76, 99, 0);
              font-size: 22px;
              font-family: 'Marck Script', cursive;
              outline: none;
              resize: none;
            }
          
            span {
              display: flex;
              justify-content: space-between;
              width: 12.5%;
              color: black;
              font-size: 20px;
              i {
                cursor: pointer;
              }
            }
          }
        }
      }
    }
</style>
