
<template>
    <div class="container-fluid py-1 text-center">
        <h3>Приём заявок</h3>
        <hr />
    </div>
    <h5 class="primary mx-4">Исполнитель</h5>
    <div id="footer" class="container-fluid mx-2 row">
        <select id="inputUser" v-model="id" class="form-control col-md-4 mb-1 " autofocus @change="chEntry">
            <option v-for="(value,key) in users" v-bind:key="key" v-bind:value="value.id">
                {{value.name}}
            </option>
        </select>

        <button id="btnEdit" class="btn btn-secondary col-md-1 mb-1 mx-3"
                @click="setUser">Выбрать</button>
        <button id="btnNew" class="btn btn-secondary col-md-1 mb-1 mx-3"> Новый </button>
        <button id="btnReturn" class="btn btn-secondary col-md-2 mb-1 mx-3" disabled="true"
                @click="varUser">Сменить исполнителя</button>
        
    </div>
        <hr />
    <div>
        <h5 class="primary mx-4">Список заявок</h5>
        <select id="spWork" v-model="work" class="form-control col-md-10 mb-3 mx-4" size="3" @change="viewWork" disabled="true">
            <option v-for="work in outWork" v-bind:key="work" v-bind:value="work">
                {{work.data}} | {{work.typeApp}} :  {{work.txtApp}}
            </option>
        </select>
    </div>

    <div class="container-fluid" id="btnD">
        <button id="btnRed" class="btn btn-secondary col-md-2 mb-1 mx-3" disabled="true"
                @click="editDecl">Редактировать</button>
        <button id="btnN" class="btn btn-secondary col-md-2 mb-1 mx-3" disabled="true">Новая заявка</button>
        <button id="btnSave" class="btn btn-secondary col-md-2 mb-1 mx-3" disabled="true"
                @click="saveDecl">Сохранить</button>
    </div>
    
    <div class="container-fluid" id="view">
        <hr />
        <h5 class="text-center">Заявки</h5>
        <br />
        <div class="row">
            <label for="dataDec" class="mx-3">Дата</label>
            <input type="text" class="form-control col-md-2 mb-1" id="dataDec" value="" required disabled = "true"/>
            <label for="typeDec" class="mx-3">Тип заявки</label>
            <input type="text" class="form-control col-md-6 mb-1" id="typeDec" value="" required disabled = "true"/>
        </div>
        <div class="row">
            <label for="statusDec" class="mx-3">Статус</label>
            <input type="text" class="form-control col-md-2 mb-1" id="statusDec" value="" required disabled = "true"/>
            <label for="authorDec" class="mx-3">Автор заявки</label>
            <input type="text" class="form-control col-md-5 mb-1" id="autDec" value="" required disabled = "true"/>
        </div>        
        <label for="txtDec">Текст </label>
        <input type="text" class="form-control col-md-10 mb-1 mx-3" id="txtDec" value="" required disabled = "true"/>
        <hr />
    </div>





</template>

<script>
 // в JSON ввсести поле uniqId - уникальный номер заявки, чтобы по ней можно было сохранять

    import spisok from "/App.json"
   // import accUser from "/menuApp.json"

 export default {
  name: "App",
  components: {

        },
  data() {
      return {
          selected: 1,
          id: 0,
          outWork: '',
          work : '',
          users: [
              { id: 1, name: 'Иванов И.И.'},
              { id: 2, name: 'Петров С.П.'},
              { id: 3, name: 'Квантов А.А.'}
          ]
      }
  },
  methods: {
      chEntry() {
          // значение выбранное из выпадающего списка записывается в id и фильтруется JSON
          this.outWork = spisok.
              filter(work => work.userId === this.id)
      },
      viewWork() {
          document.getElementById('dataDec').value = this.work.data
          document.getElementById('typeDec').value = this.work.typeApp
          document.getElementById('statusDec').value = this.work.statusApp
          document.getElementById('txtDec').value = this.work.txtApp
          document.getElementById('autDec').value = this.work.autApp
      },
      varUser() {
          if (this.id !== 0) {
              document.getElementById('inputUser').disabled = false
              document.getElementById('btnNew').disabled = false
              document.getElementById('btnEdit').disabled = false
              document.getElementById('btnReturn').disabled = true
              document.getElementById('spWork').disabled = true
              document.getElementById('btnRed').disabled = true
              document.getElementById('btnN').disabled = true

          }

      },
      setUser() {
          if (this.id !== 0) {
              document.getElementById('inputUser').disabled = true
              document.getElementById('btnNew').disabled = true
              document.getElementById('btnEdit').disabled = true
              document.getElementById('btnReturn').disabled = false
              document.getElementById('spWork').disabled = false
              document.getElementById('btnRed').disabled = false
              document.getElementById('btnN').disabled = false
          }
      },
      editDecl() {
          document.getElementById('spWork').disabled = true
          document.getElementById('btnN').disabled = true
          document.getElementById('btnReturn').disabled = true
          document.getElementById('btnRed').disabled = true
          document.getElementById('btnSave').disabled = false

          document.getElementById('dataDec').disabled = false
          document.getElementById('typeDec').disabled = false
          document.getElementById('statusDec').disabled = false
          document.getElementById('txtDec').disabled = false
          document.getElementById('autDec').disabled = false
      },
      saveDecl() {

          // спросить сохранять изменения или нет

          document.getElementById('spWork').disabled = false
          document.getElementById('btnN').disabled = false
          document.getElementById('btnReturn').disabled = false
          document.getElementById('btnRed').disabled = false
          document.getElementById('btnSave').disabled = true

          document.getElementById('dataDec').disabled = true
          document.getElementById('typeDec').disabled = true
          document.getElementById('statusDec').disabled = true
          document.getElementById('txtDec').disabled = true
          document.getElementById('autDec').disabled = true
      }
  }
};
</script>

