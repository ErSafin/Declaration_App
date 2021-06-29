
<template>
    <div class="container-fluid py-1 text-center">
        <h3>Приём заявок</h3>
        <hr />
    </div>
    <h5 class="primary mx-4">Исполнитель</h5>
    <div id="footer" class="container-fluid mx-2 row">
        <select id="inputUser" v-model="users.id" class="form-control col-md-4 mb-1 " autofocus @change="chEntry(users.id)" selected>
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


            <div class="container-fluid" id="btnD">
                <button id="btnRed" class="btn btn-secondary col-md-2 mb-1 mx-3" disabled
                        @click="editDecl">
                    Редактировать
                </button>
                <button id="btnN" class="btn btn-secondary col-md-2 mb-1 mx-3" disabled
                        @click="newDecl">
                    Новая заявка
                </button>
                <button id="btnSave" class="btn btn-secondary col-md-2 mb-1 mx-3" disabled
                        @click="saveDecl(parseInt(users.id))">
                    Сохранить
                </button>
            </div>
        </div>
    
    <div class="container-fluid" id="view">
        <hr />
        <h5 class="text-center">Заявки</h5>
        <br />
        <div class="row">
            <label for="dataDec" class="mx-3">Дата</label>
            <input type="text" class="form-control col-md-2 mb-1" id="dataDec" v-model="dataDec" required disabled/>
            <label for="typeDec" class="mx-3">Тип заявки</label>
            <input type="text" class="form-control col-md-6 mb-1" id="typeDec" v-model="typeDec" required disabled/>
        </div>
        <div class="row">
            <label for="statusDec" class="mx-3">Статус</label>
            <input type="text" class="form-control col-md-2 mb-1" id="statusDec" v-model="statusDec" required disabled/>
            <label for="authorDec" class="mx-3">Автор заявки</label>
            <input type="text" class="form-control col-md-5 mb-1" id="autDec" v-model="autDec" required disabled/>
        </div>        
        <label for="txtDec">Текст </label>
        <input type="text" class="form-control col-md-10 mb-1 mx-3" id="txtDec" v-model="txtDec" required disabled/>
        <hr />
    </div>





</template>

<script>

 // TODO: валидация наличия файлов JSON

 import spisok from "/App.json"
 import newuser from "/menuApp.json"

 export default {
  name: "App",
  components: {
      
        },
  data() {
      return {
          selected: 0,
          outWork: '',
          work: '',
          newSpisok: spisok,
          users: newuser,
          isEdit: false,
          indEdit: 0,
          nUniqId: 0,
          nullArray: {
              "userId": 0,
              "data": "",
              "typeApp": "",
              "statusApp": "",
              "autApp": "",
              "txtApp": "",
              "uniqId": 0
          },
          dataDec: '',
          typeDec: '',
          statusDec: '',
          autDec: '',
          txtDec: '',
          uniqDec: 0,
          userDec: 0,
          addJson: [],
          errors: []
          
      }
  },
  methods: {
      checkErrors() {
          this.errors = []
          if (!this.dataDec) {
              this.errors.push('Не введена дата.')
          }
          if (!this.typeDec) {
              this.errors.push ('Не введен тип заявки.')
          }
          if (!this.errors.length) {
              return true
          }
          //e.preventDefault()

      },

      chEntry(tid) {
          // значение выбранное из выпадающего списка записывается в id и фильтруется JSON
          if (!isNaN(tid) || tid === 0) {
              this.outWork = this.newSpisok.
                  filter(work => work.userId === parseInt(tid))
          }
      },
      // Отображение текущей заявки в полях input 
      viewWork(arrWork) {
          this.dataDec = arrWork.data
          this.typeDec = arrWork.typeApp
          this.statusDec = arrWork.statusApp
          this.autDec = arrWork.autApp
          this.txtDec = arrWork.txtApp
      },
      boolSet(boolId, blockID) {
          let bT = false
          let bF = true
          if (boolId === 1) {
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
              this.boolSet(2, 1) // блокировка и разблокировка полей и кнопок
          } else {
              alert("Выберите исполнителя.")
          }

      },
      // Выбор пользователя
      setUser(suTid) {
          if (!isNaN(suTid) || suTid === 0) {
              this.boolSet(1, 1) // блокировка и разблокировка полей и кнопок
              document.getElementById('spWork').autofocus = true
          } else {
              alert("Выберите исполнителя.")
          }
      },
      // Новый пользователь
      newUser() {
          const newU = prompt("Введите нового исполнителя ФИО");
          alert(newU)
          // TODO: запись в JSON menuApp

      },
      // Редактирование заявки
      editDecl() {
          this.boolSet(1, 2) // блокировка и разблокировка полей и кнопок
          this.indEdit = this.newSpisok.findIndex((ie) => { return parseInt(ie.uniqId) === parseInt(this.work.uniqId) })
          this.nUniqId = parseInt(this.work.uniqId)
          this.isEdit = true
          
      },
      // Сохранение заявки
      saveDecl(numId) {
          const isSave = confirm("Сохранить изменения?");
          if (isSave) {

              //debugger

              // TODO: добавить валидацию на пустые поля и дату
              this.nullArray.userId = numId
              this.nullArray.data = this.dataDec
              this.nullArray.typeApp = this.typeDec
              this.nullArray.statusApp = this.statusDec
              this.nullArray.txtApp = this.txtDec
              this.nullArray.autApp = this.autDec

                                          
              // перенос данных в рабочий массив newSpisok, НО БЕЗ сохранения в JSON-файл

              console.log(this.indEdit)

              // TODO: при выборе Иванов и 1 позицию в массиве =0 и при редактировании всё норм, а при добавлении нового
              //        изменяет позицию = 0 и добавляет тоже самое в конец списка т.е. пушит
              //   вероятно проблема с значением uniqId

              // всё это делает при присваивании значения в nullArray еще не доходя до проверки редактирования
              // - может от nullArray отказаться ?



              console.log(this.nullArray)
              console.log(this.newSpisok)
              console.log(this.isEdit)


              if (this.isEdit) {  // редактирование
                  this.nullArray.uniqId = this.nUniqId
                  this.newSpisok[this.indEdit] = this.nullArray
          

              } else { // новый
                  this.nullArray.uniqId = parseInt(Date.now())
                  this.newSpisok.push(this.nullArray)
          
              }
              this.chEntry(this.users.id)
              this.isEdit = null

              console.log(this.newSpisok)

              // TODO: надо перенести addJson в конец файла App.json
              // this.addJson = JSON.stringify([this.newSpisok])
              // console.log(this.addJson)


              
          } else {
              //TODO: надо передать фокус на id="spWork"
              document.getElementById('spWork').autofocus=true
          }
         this.boolSet(2, 2) // блокировка и разблокировка полей и кнопок
      },

      // Новая заявка
      newDecl() {
          this.boolSet(1, 2) // блокировка и разблокировка полей и кнопок
          this.isEdit = false

          this.dataDec = ''
          this.typeDec = ''
          this.statusDec = ''
          this.txtDec = ''
          this.autDec = ''
      //    this.indEdit = this.newSpisok.length
      //    console.log(this.indEdit)
      },
      procSave() {
          


     
      }
  }
};
</script>

