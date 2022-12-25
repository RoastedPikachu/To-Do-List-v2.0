<template>
  <HeaderComp/>
  <section class="darkTheme">
    <div id="informationBlock">
      <span id="informationBlock-referenceInformation">
        <p>Сервис</p>
        <p>Цена подписки</p>
        <p>Частота платежа</p>
      </span>
      <ul type="none">
        <li v-for="subscribe of subscribes" :key="subscribe.id">
          <input type="text" :value="subscribe.service" maxlength="20" disabled>
          <input type="text" :value="subscribe.cost" maxlength="10" disabled>
          <input type="text" :value="subscribe.periodicity" maxlength="15" disabled>
          <i id="editIcon" class="fa-solid fa-pen" @click="editSubscribe($event)"></i>
          <i id="confirmEditIcon" class="fa-solid fa-check inactive" @click="confirmEditSubscribe($event)"></i>
          <i class="fa-solid fa-xmark" @click="deleteSubscribe($event)"></i>
        </li>
      </ul>
      <span id="informationBlock-addBlock">
        <i class="fa-solid fa-plus" @click="modalInteraction"></i>
        <p>Добавить подписку</p>
      </span>
    </div>  
    <aside></aside>
  </section>
  <div id="addSubscribeBlock" v-if="visibility" @keydown="isEnter($event)">
    <h3>Добавление подписки</h3>
    <input type="text" placeholder="Введите сервис подписки" maxlength="20" v-model="subscribeService">
    <input type="text" placeholder="Введите цену подписки" maxlength="10" v-model="subscribeCost">
    <input type="text" placeholder="Введите частоту платежа" maxlength="15" v-model="subscribePeriodicity">
    <input type="date" placeholder="Введите дату окончания подписки" v-model="subscribeExpirationDate">
    <span id="buttonsBlock">
      <button id="buttonsBlock-cancel" @click="modalInteraction">Отменить</button>
      <button id="buttonsBlock-add" @click="addSubscribe">Добавить подписку</button> 
    </span>
  </div>
  <FooterComp/>
</template>

<script>
import HeaderComp from './components/HeaderComp.vue';
import FooterComp from './components/FooterComp.vue';

export default {
  name: 'App',
  data(){
    return {
      subscribes: JSON.parse(localStorage.getItem('subscribes')) || [],
      index: JSON.parse(localStorage.getItem('index')) || 1,
      id: 0,
      editIndex: 0,
      subscribeService: '',
      subscribeCost: '',
      subscribePeriodicity: '',
      subscribeExpirationDate: '',
      visibility: false,
    }
  },
  methods: {
    modalInteraction() {
      this.visibility = !this.visibility;
      document.querySelector('header').classList.toggle('modalActive');
      document.querySelector('section').classList.toggle('modalActive');
      document.querySelector('footer').classList.toggle('modalActive');
    },
    addSubscribe() {
      let subscribe = {
        id: this.index,
        service: (this.subscribeService[0].toUpperCase() + this.subscribeService.slice(1)).trim(),
        cost: (this.subscribeCost[0].toUpperCase() + this.subscribeCost.slice(1)).trim(),
        periodicity: (this.subscribePeriodicity[0].toUpperCase() + this.subscribePeriodicity.slice(1)).trim(),
      } 

      this.index++;
      this.subscribeService = '';
      this.subscribeCost = '';
      this.subscribePeriodicity = '';
      this.subscribeExpirationDate = '';

      this.subscribes.push(subscribe);
      localStorage.setItem('index', JSON.stringify(this.index));
      localStorage.setItem('subscribes', JSON.stringify(this.subscribes));
      this.modalInteraction();
    },
    editSubscribe(event) {
      event.target.classList.toggle('inactive');
      event.target.nextElementSibling.classList.toggle('inactive');

      event.target.parentElement.children[0].removeAttribute('disabled');
      event.target.parentElement.children[1].removeAttribute('disabled');
      event.target.parentElement.children[2].removeAttribute('disabled');

      this.subscribes.forEach((item, index) => {
        if(item.service == event.target.parentElement.children[0].value) {
          this.editIndex = index;
          this.id = item.id;
        }
      });
    },
    confirmEditSubscribe(event) {
      event.target.classList.toggle('inactive');
      event.target.previousElementSibling.classList.toggle('inactive');

      event.target.parentElement.children[0].setAttribute('disabled', true);
      event.target.parentElement.children[1].setAttribute('disabled', true);
      event.target.parentElement.children[2].setAttribute('disabled', true);

      let serviceText = event.target.parentElement.children[0].value;
      let costText = event.target.parentElement.children[1].value;
      let periodicityText = event.target.parentElement.children[2].value;

      let subscribe = {
        id: this.id,
        service: (serviceText[0].toUpperCase() + serviceText.slice(1)).trim(),
        cost: (costText[0].toUpperCase() + costText.slice(1)).trim(),
        periodicity: (periodicityText[0].toUpperCase() + periodicityText.slice(1)).trim()
      }

      this.subscribes.splice(this.editIndex, 1, subscribe);
      localStorage.setItem('subscribes', JSON.stringify(this.subscribes));
    },
    deleteSubscribe(event) {
      let valueService = event.target.parentElement.children[0].value;

      this.subscribes = this.subscribes.filter(item => item.service != valueService);

      localStorage.setItem('subscribes', JSON.stringify(this.subscribes));
      localStorage.setItem('index', JSON.stringify(this.index));
      event.target.parentElement.remove();
    },
    isEnter(event) {
      if(event.keyCode == 13) {
        this.addSubscribe();
      }
    }
  },
  components: {
    HeaderComp,
    FooterComp
  }
}
</script>

<style lang="scss">
  @import url('https://fonts.googleapis.com/css2?family=Montserrat+Alternates:wght@300;400;500;600;700&family=Montserrat:ital,wght@0,400;0,500;0,600;1,300&display=swap');

  * {
    margin:0;
    padding:0;
  }

  section {
    display: flex;
    min-height: 600px;
    #informationBlock {
      display: flex;
      flex-direction: column;
      margin-top: 50px;
      margin-left: 5%;
      width: 65%;
      #informationBlock-referenceInformation {
        display: flex;
        justify-content: space-between;
        align-items: flex-start;
        padding: 0% 15% 0% 5%;
        width: 80%;
        height: 50px;
        font-size: 26px;
        font-weight: 500;
        font-family: 'Montserrat Alternates', sans-serif;
      }
      ul {
        width: 100%;
        li {
          display: flex;
          justify-content: space-around;
          align-items: center;
          margin-top: 50px;
          font-size: 22px;
          font-family: 'Montserrat', sans-serif;
          p {
            width: 20%;
            text-align: center;
            word-wrap: break-word;
          }
          p:nth-child(2) {
            width: 30%;
          }
          p:nth-child(3) {
            width: 40%;
          }
          input {
            width: 20%;
            font-size: 22px;
            font-family: 'Montserrat', sans-serif;
            border: 0;
            outline: none;
            text-align: center;
          }
          input:nth-child(2) {
            width: 30%;
          }
          input:nth-child(3) {
            width: 40%;
          }
          i {
            width: 5%;
            font-size: 24px;
            cursor: pointer;
            transition: 500ms ease;
          }
          i:hover {
            color: #ffe100;
          }
          .inactive {
            display: none;
          }
        }
      }
      #informationBlock-addBlock {
        display: flex;
        align-items: center;
        margin-top: 50px;
        height: 50px;
        i {
          margin-left: 5%;
          width: 50px;
          font-size: 44px;
          cursor: pointer;
          transition: 500ms ease;
        }
        i:hover {
          color: #ffe100;
        }
        p {
          margin-left: 20px;
          font-size: 22px;
          font-family: 'Montserrat', sans-serif;
        }
      }
    }
    aside {
      margin-top: 40px;
      width: 25%;
    }
  }

  #addSubscribeBlock {
      position: absolute;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      left: 50%;
      top: 50%; 
      margin-left: -250px; 
      margin-top: -250px;
      width: 500px;
      height: 500px;
      background-color: #272727;
      color: #fffcf4;
      border: 3px solid #fffcf4;
      border-radius: 20px;
      h3 {
        font-size: 24px;
        font-family: 'Montserrat', sans-serif;
      }
      input {
        margin-top: 40px;
        padding-left: 10px;
        width: 80%;
        height: 35px;
        background-color: #fffcf4;
        border: 0;
        border-radius: 5px;
        color: #272727;
        font-size: 20px;
        font-family: 'Montserrat', sans-serif;
        outline: none;
      }
      #buttonsBlock {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-top: 50px;
        margin-left: 13%;
        width: 70%;
        button {
          height: 40px;
          background-color: #272727;
          border: 2px solid #fffcf4;
          border-radius: 10px;
          color: #fffcf4;
          font-size: 18px;
          font-family: 'Montserrat', sans-serif;
          cursor: pointer;
          transition: 500ms ease;
        }
        button:active {
          transform: translateY(5px);
        }
        #buttonsBlock-cancel {
          width: 120px;
        }
        #buttonsBlock-cancel:hover {
          background-color: rgb(200, 21, 21);
        }
        #buttonsBlock-add {
          width: 210px;
        }
        #buttonsBlock-add:hover {
          background-color: rgb(32, 147, 32);
        }
      }
    }

.modalActive {
  pointer-events: none;
  filter: brightness(0.5);
}

.darkTheme {
  background-color: #272727;
  div {
    #informationBlock-referenceInformation {
      border: 0px solid #fffcf4;
      border-width: 0px 0px 3px;
      color: #fffcf4;
    }
    ul {
      li {
        color: #fffcf4;
        input {
          background-color: #272727;
          color: #fffcf4;
        }
      }
    }
    #informationBlock-addBlock {
      color: #fffcf4;
    }
  }
  aside {
    border: 0px solid #fffcf4;
    border-width: 0px 0px 0px 3px;
  }
}

.lightTheme {
  background-color: #fffcf4;
  div {
    #informationBlock-referenceInformation {
      border: 0px solid #272727;
      border-width: 0px 0px 3px;
      color: #272727;
    }
    ul {
      li {
        color: #272727;
        input {
          background-color: #fffcf4;
          color: #272727;
        }
      }
    }
    #informationBlock-addBlock {
      color: #272727;
    }
  }
  aside {
    border: 0px solid #272727;
    border-width: 0px 0px 0px 3px;
  }
}

</style>
