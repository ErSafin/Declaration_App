<template>
     
    <h4 class="py-1 text-center">Приём заявок</h4>
    <hr />
    <user-item @create="showWorks"/>
          
    <div>
        <button id="btnEdit" class="btn btn-secondary col-md-2 my-2 mb-1 mx-5"
                @click="setUser(parseInt(id))">
            Выбрать
        </button>
        <button id="btnNew" class="btn btn-secondary col-md-2 mb-1 my-2"
                @click="newUser()">
            Новый
        </button>
        <button id="btnReturn" class="btn btn-secondary col-md-3 mb-1 my-2 mx-3" disabled
                @click="varUser(parseInt(id))">
            Сменить исполнителя
        </button>
    </div>
        <hr />
        <div>
            <label class="head1 mx-4 col-md-5">Список заявок</label> <span class="sort md-4 " id="lbl1" hidden>Сортировка</span>
             <a href="#" id="sortData" class="sort mx-4" @click="sortData('data')" hidden>по дате</a>  
             <a href='#' id="sortStat" class="sort" @click="sortData('statusApp')" hidden>по статусу</a>
            <list-request v-bind:outWork="outWork" @work="showView"/>
            


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
                        @click="saveDecl(parseInt(id))">
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
 
 import UserItem from '@/components/UserItem'
 import ListRequest from '@/components/ListRequest'

 export default {
  name: "App",
        components: {
            UserItem, ListRequest
        },
  data() {
      return {
          id: 0,
          outWork: '',
          work: '',
          newSpisok: spisok,
          isEdit: null,
          indEdit: 0,
          nUniqId: 0,
          dataDec: '',
          typeDec: '',
          statusDec: '',
          autDec: '',
          txtDec: '',
          uniqDec: 0,
          userDec: 0,
          newJson: [],
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
    
      showWorks(presentId) {
          // значение выбранное из выпадающего списка записывается в id и фильтруется JSON
          if (!isNaN(presentId) || presentId === 0) {
              this.id = presentId
              this.outWork = this.newSpisok.
                  filter(work => work.userId === parseInt(presentId))
          }
      },
      // Отображение текущей заявки в полях input 
      showView(arrWork) {
          this.dataDec = arrWork.data
          this.typeDec = arrWork.typeApp
          this.statusDec = arrWork.statusApp
          this.autDec = arrWork.autApp
          this.txtDec = arrWork.txtApp
          this.nUniqId = arrWork.uniqId
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
              document.getElementById('sortData').hidden = bF
              document.getElementById('sortStat').hidden = bF
              document.getElementById('lbl1').hidden = bF
              document.getElementById('btnRed').disabled = bF
              document.getElementById('btnN').disabled = bF
          } else {
              document.getElementById('spWork').disabled = bT
              document.getElementById('sortData').hidden = bT
              document.getElementById('sortStat').hidden = bT
              document.getElementById('lbl1').hidden = bT
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
          if (vuTid !== 0 & !isNaN(vuTid)) {
              // формируется JSON
              this.newJson = JSON.stringify([this.newSpisok])

              // TODO: надо перезаписать файл App.json
              //console.log(this.newJson)

              this.boolSet(2, 1) // блокировка и разблокировка полей и кнопок
          } else {
              alert("Выберите исполнителя.")
          }
      },
      // Выбор пользователя
      setUser(suTid) {
          if (suTid !== 0 & !isNaN(suTid)) {
              this.boolSet(1, 1) // блокировка и разблокировка полей и кнопок
              //TODO : передать фокус на id = spWork

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
          if (parseInt(this.nUniqId) !== 0) {
              this.boolSet(1, 2) // блокировка и разблокировка полей и кнопок
              this.indEdit = this.newSpisok.findIndex((ie) => { return parseInt(ie.uniqId) === parseInt(this.nUniqId) })
              this.nUniqId = parseInt(this.work.uniqId)
              this.isEdit = true
          } else {
              alert("Выберите элемент 'СПИСКА ЗАЯВОК'")
          }
          
      },
      // Сохранение заявки
      saveDecl(numId) {
          const nullArray = { 
              userId: 0,
              data: "",
              typeApp: "",
              statusApp: "",
              autApp: "",
              txtApp: "",
              uniqId: 0
          }
          let isFlag = false

          if (this.typeDec.length === 0 | this.statusDec.length === 0 |
              this.txtDec.length === 0 | this.autDec.length === 0) {

              const isEmptyQ = confirm("Остались пустые поля. Сохранить?")
              if (isEmptyQ) {
                  isFlag = true
              }
          } else {
              const isSave = confirm("Сохранить изменения?");
              if (isSave) {
                  isFlag = true
              }
          }
          
          // перенос данных в рабочий массив newSpisok, НО БЕЗ сохранения в JSON-файл
          if (isFlag) {
                  nullArray.userId = numId
                  nullArray.data = this.dataDec
                  nullArray.typeApp = this.typeDec
                  nullArray.statusApp = this.statusDec
                  nullArray.txtApp = this.txtDec
                  nullArray.autApp = this.autDec


              
              if (this.isEdit) {  // редактирование
                  nullArray.uniqId = this.nUniqId
                  this.newSpisok[this.indEdit] = nullArray
              } else { // новый
                  nullArray.uniqId = parseInt(Date.now())
                  this.newSpisok.push(nullArray)
              }
         //     this.showWorks(parseInt(this.id))
              this.isEdit = null
          } else {


              //TODO: надо передать фокус на id="spWork"
              
          }
         this.showWorks(parseInt(this.id))
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
      },
      sortData(inputData) {

          this.outWork.sort(() => {
          
              if ("a." + inputData > "b." + inputData) {
                  return 1
              }
              if ("a." + inputData < "b." + inputData) {
                  return -1
              }
              return 0
          })


      }
      

  },
  watch: {
      //TODO: просмотр действий - время редактирования, время бездействия

  }
};
</script>

<style>
    .head1 {
        font-size: 1.3em
    }

</style>
