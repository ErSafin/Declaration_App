
<template>
    <div class="container-fluid py-1 text-center">
        <h3>Приём заявок</h3>
        <hr />
    </div>
    <h5 class="primary mx-4">Исполнитель</h5>
    <div id="footer" class="container-fluid mx-2 row">
        <select id="inputUser" v-model="users.id" class="form-control col-md-4 mb-1 " autofocus @change="chEntry(users.id)">
            <option v-for="(value,key) in users" v-bind:key="key" v-bind:value="value.id">
                {{value.name}}
            </option>
        </select>

        <button id="btnEdit" class="btn btn-secondary col-md-1 mb-1 mx-3"
                @click="setUser(parseInt(users.id))">Выбрать</button>
        <button id="btnNew" class="btn btn-secondary col-md-1 mb-1 mx-3"
                @click="newUser()"> Новый </button>
        <button id="btnReturn" class="btn btn-secondary col-md-2 mb-1 mx-3" disabled
                @click="varUser(parseInt(users.id))">Сменить исполнителя</button>
        
    </div>
        <hr />
    <div>
        <h5 class="primary mx-4">Список заявок</h5>
        <select id="spWork" v-model="work" class="form-control col-md-10 mb-3 mx-4" size="3" @change="viewWork(work)" disabled>
            <option v-for="work in outWork" v-bind:key="work" v-bind:value="work">
                {{work.data}} | {{work.typeApp}} :  {{work.txtApp}}
            </option>
        </select>
    </div>

    <div class="container-fluid" id="btnD">
        <button id="btnRed" class="btn btn-secondary col-md-2 mb-1 mx-3" disabled
                @click="editDecl">Редактировать</button>
        <button id="btnN" class="btn btn-secondary col-md-2 mb-1 mx-3" disabled>Новая заявка</button>
        <button id="btnSave" class="btn btn-secondary col-md-2 mb-1 mx-3" disabled
                @click="saveDecl">Сохранить</button>
    </div>
    
    <div class="container-fluid" id="view">
        <hr />
        <h5 class="text-center">Заявки</h5>
        <br />
        <div class="row">
            <label for="dataDec" class="mx-3">Дата</label>
            <input type="text" class="form-control col-md-2 mb-1" id="dataDec" value="" required disabled/>
            <label for="typeDec" class="mx-3">Тип заявки</label>
            <input type="text" class="form-control col-md-6 mb-1" id="typeDec" value="" required disabled/>
        </div>
        <div class="row">
            <label for="statusDec" class="mx-3">Статус</label>
            <input type="text" class="form-control col-md-2 mb-1" id="statusDec" value="" required disabled/>
            <label for="authorDec" class="mx-3">Автор заявки</label>
            <input type="text" class="form-control col-md-5 mb-1" id="autDec" value="" required disabled/>
        </div>        
        <label for="txtDec">Текст </label>
        <input type="text" class="form-control col-md-10 mb-1 mx-3" id="txtDec" value="" required disabled/>
        <hr />
    </div>





</template>

<script>
 // в JSON ввсести поле uniqId - уникальный номер заявки, чтобы по ней можно было сохранять
    
    import spisok from "/App.json"
    import newuser from "/menuApp.json"

 export default {
  name: "App",
  components: {
      
        },
  data() {
      return {
          selected: 1,
          outWork: '',
          work: '',
          users: newuser
          
      }
  },
  methods: {
      chEntry(tid) {
          // значение выбранное из выпадающего списка записывается в id и фильтруется JSON
          this.outWork = spisok.
              filter(work => work.userId === parseInt(tid))
      },
      // Отображение текущей заявки в полях input 
      viewWork(arrWork) {
          
          document.getElementById('dataDec').value = arrWork.data
          document.getElementById('typeDec').value = arrWork.typeApp
          document.getElementById('statusDec').value = arrWork.statusApp
          document.getElementById('txtDec').value = arrWork.txtApp
          document.getElementById('autDec').value = arrWork.autApp
      
      },
      boolSet(boolId, blockID) {
          let bT = false
          let bF = true
          if (boolId === true) {
              bT = true
              bF = false
          }
          if (blockID === 1) {
              document.getElementById('inputUser').disabled = bT
              document.getElementById('btnNew').disabled = bT
              document.getElementById('btnEdit').disabled = bT
              document.getElementById('btnReturn').disabled = bF
              document.getElementById('spWork').disabled = bF
              document.getElementById('btnRed').disabled = bF
              document.getElementById('btnN').disabled = bF
          } else {
              document.getElementById('spWork').disabled = bT
              document.getElementById('btnN').disabled = bT
              document.getElementById('btnReturn').disabled = bT
              document.getElementById('btnRed').disabled = bT
              document.getElementById('btnSave').disabled = bF

              document.getElementById('dataDec').disabled = bF
              document.getElementById('typeDec').disabled = bF
              document.getElementById('statusDec').disabled = bF
              document.getElementById('txtDec').disabled = bF
              document.getElementById('autDec').disabled = bF
          }
          
      },
      // Смена пользователя
      varUser(vuTid) {
          if (!isNaN(vuTid) || vuTid === 0) {
              this.boolSet(false, 1) // блокировка и разблокировка полейи кнопок
          }

      },
      // Выбор пользователя
      setUser(suTid) {
          if (!isNaN(suTid) || suTid === 0) {
              this.boolSet(true, 1) // блокировка и разблокировка полейи кнопок
          }
      },
      // Новый пользователь
      newUser() {

      },
      // Редактирование заявки
      editDecl() {
          this.boolSet(true, 2) // блокировка и разблокировка полейи кнопок
          
      },
      // Сохранение заявки
      saveDecl() {
          this.boolSet(false, 2) // блокировка и разблокировка полейи кнопок

          //TODO: спросить сохранять изменения или нет


      }
  }
};
</script>

