<template>
  <div class="form" autocomplete="off">
    <form action="" name="Личные данные">
      <div class="row">{{formData.name}}</div>
      <h1>Личные данные</h1>
      <div class="row name">
        <label for="lastName">Фамилия
          <input type="text" name="lastName" class="personLastName" v-model="formData.secondName">
        </label>
        <label for="name">Имя
          <input type="text" name="name" class="personName" v-model="formData.name">
        </label>
        <label for="fathersName">Отчество
          <input type="text" name="fathersName" class="personFathersName" v-model="formData.fathersName">
        </label>
      </div>
      <div class="row">
        <label for="dateOfBirth" class="dateOfBirth">Дата рождения
          <input type="date" name="dateOfBirth" class="dateOfBirth__input" v-model="formData.dateOfBirth">
        </label>
      </div>
      <div class="row">
        <label for="email" class="email">E-mail
          <input type="email" name="email" class="email__input" v-model="formData.email">
        </label>
      </div>
      <div class="row">
        <h2>Пол</h2>
        <label>Мужской
          <input type="radio" class="radio" name="genderSelector" value="male" v-model="formData.gender">
        </label>
        <label>Женский
          <input type="radio" class="radio" name="genderSelector" value="female" v-model="formData.gender">
        </label>
      </div>
      <h2>Паспортные данные</h2>
      <div class="row">
        <div class="nationality-selector"  v-click-outside="hide">
          <label for="nationality">Гражданство</label>
          <span class="arrow-nationality" @click="isDropdownOpen = true">&#9660;</span>
          <input type="text" 
          v-model="searchWord"
          id="nationality"
          @focus="isDropdownOpen = true"
         
          />
          <div
          v-if="isDropdownOpen"
          class="nationality-selector__dropdown disable-scrollbars"
          >
          <ul>
            <li
            v-for="country in countries"
            :key="country"
            @click="countrySelected(country)"
            >
            {{country}}
            </li>
          </ul>
          </div>
        </div>
      </div>
      <div class="row" v-if="this.formData.nationality == 'Russia' || this.formData.nationality == ''">
        <label for="passSer" >Серия паспорта
          <input type="text" name="passSer" minlength="4" maxlength="4" v-model="formData.passSer" class="passSer">
        </label>
        <label for="passNum">Номер паспорта
          <input type="text" name="passNum" minlength="6" maxlength="6" v-model="formData.passNum" class="passNum">
        </label>
        <label for="passDate">Дата выдачи
          <input type="date" name="passDate" v-model="formData.passDate">
        </label>
      </div>
      <div class="row foreigner" v-else>
        <div class="foreigner__container">
          <label for="foreignLastName" >Фамилия на латинице
          <input type="text" name="foreignName" class="foreignLastName" v-model="formData.foreignLastName">
        </label>
        <label for="foreignName">Имя на латинице
          <input type="text" name="foreignName" class="foreignName" v-model="formData.foreignName">
          
        </label>
        </div>
        <label>Иностранцы заполняют латинскими буквами. Например, Ivanov Ivan</label>
        <div class="foreignPass">
          <div class="foreigner__passNum">
            <label for="passNum">Номер паспорта
            <input type="text" name="passNum" class="passNum" v-model="formData.passNum">
          </label>
          <div class="passCountry-selector"  v-click-outside="hidePassCountry">
            <label for="passCountry">Страна выдачи</label>
            <span class="arrow" @click="isDropdownOpenCountrySelector = true">&#9660;</span>
            <input type="text" 
            id="passCountry"
            @focus="isDropdownOpenCountrySelector = true"
            />
            <div
            v-if="isDropdownOpenCountrySelector"
            class="passCountry-selector__dropdown disable-scrollbars"
            >
            <ul>
              <li
              v-for="country in countries"
              :key="country"
              @click="passCountrySelected(country)"
              >
              {{country}}
              </li>
            </ul>
            </div>
          </div>

          <div class="passType-selector"  v-click-outside="hidePassType">
            <label for="passType">Тип паспорта </label>
            <input type="text" 
            id="passType"
            @focus="isDropdownOpenPassType = true"
            />  
            <div
            v-if="isDropdownOpenPassType"
            class="passType-selector__dropdown"
            >
            <ul>
              <li
              v-for="type in passTypes"
              :key="type"
              @click="passTypeSelected(type)"
              >
              {{type}}
              </li>
            </ul>
            </div>
          </div>
          </div>
        </div>
      </div>
      <div class="row">
        <h2>Меняли ли фамилию или имя?</h2>
        <label>Да
          <input type="radio" class="radio" name="nameChanged" value="yes" v-model="nameChanged">
        </label>
        <label>Нет
          <input type="radio" class="radio" name="nameChanged" value="no" v-model="nameChanged">
        </label>
      </div>
      <div class="row" v-if="nameChanged == 'no'"></div>
      <div class="row" v-else>
        <label for="prevLastName" class="prevLastName">Фамилия
          <input type="text" name="prevLastName" v-model="formData.prevLastName">
        </label>
        <label for="prevLastName" class="prevLastName">Имя
          <input type="text" name="prevName" class="prevName" v-model="formData.prevName">
        </label>
      </div>
      <input type="submit" value="Отправить" @click.prevent="confirm">
    </form>
  </div>
</template>

<script>
import citizenships from "../assets/data/citizenships.json";
import ClickOutside from 'vue-click-outside';
import passType from "../assets/data/passport-types.json";
// import {debounce} from "../helpers/debounce.js";
import {throttle} from "../helpers/throttle.js"
export default {
  data() {
    return {
      formData: {
      name: "",
      lastName: "",
      fathersName: "",
      dateOfBirth: "",
      email: "",
      gender: null,
      nationality: "",
      passSer: "",
      passNum: "",
      passDate: "",
      passType: "",
      prevName: "",
      foreignName: "",
      foreignLastName: "",
      prevLastName: "",
      passCountry: "",
      },
      countries: [],
      isDropdownOpen: false,
      isDropdownOpenPassType: false,
      isDropdownOpenCountrySelector: false,
      passTypes: [],
      nameChanged: 'no',
      debouncedCountry: null,
      searchWord: "",
      throttledSearchCountry: null,
    };
  },
  methods: {
    hide() {
      this.isDropdownOpen = false;
    },
    hidePassType() {
      this.isDropdownOpenPassType = false;
    },
    hidePassCountry() {
      this.isDropdownOpenCountrySelector = false;
    },
    countrySelected(country) {
      this.formData.nationality = country;
      this.hide()
    },
    passCountrySelected(country) {
      this.formData.passCountry = country;
      this.hidePassCountry()
    },
    passTypeSelected(type) {
      this.formData.passType = type;
      this.hidePassType();
    },
    confirm() {
      this.nameConfirm();
      this.foreignNameConfirm();
      this.emailConfrim(); 
      this.passNumConfirm();
      this.passSerConfirm();
      this.dateOfBirthConfirm();
      console.log(this.formData);    
    },
    getCountry(searchWord) {
      ("FETCH COUNTRY: GET COUNTRY FROM API", searchWord);
    },
    nameConfirm() {
      if(this.formData.nationality == "Russia" || this.formData.nationality == "") {
        const regexp = /[\\а-яА-я]+$/;
        const name = this.formData.name;
        const lastName = this.formData.lastName;
        const fathersName = this.formData.fathersName;
        const prevName = this.formData.prevName;
        const prevLastName = this.formData.prevLastName;
        if(!name.match(regexp)) {
          document.querySelector('.personName').classList.add('incorrect')
        }
        if(!lastName.match(regexp)) {
          document.querySelector('.personLastName').classList.add('incorrect')
        }
        if(!fathersName.match(regexp)) {
          document.querySelector('.personFathersName').classList.add('incorrect')
        }
        if(this.nameChanged == 'yes') {
          if(!prevName.match(regexp)) {
            document.querySelector('.prevName').classList.add('incorrect')
          }
          if(!prevLastName.match(regexp)) {
            document.querySelector('.prevLastName').classList.add('incorrect')
          }
        }
      }
    },
      foreignNameConfirm() {
      if(this.formData.nationality !== "Russia" && this.formData.nationality !=="") {
        const regexpRu = /[\\а-яА-я]+$/;
        const regexp = /[\\a-zA-z]+$/;
        const foreignName = this.formData.foreignName;
        const foreignLastName = this.formData.foreignLastName;
        const name = this.formData.name;
        const lastName = this.formData.lastName;
        const fathersName = this.formData.fathersName;
        if(!foreignName.match(regexp)) {
          document.querySelector('.foreignName').classList.add('incorrect');
        }
        if(!foreignLastName.match(regexp)) {
          document.querySelector('.foreignLastName').classList.add('incorrect')
        }
        if(!name.match(regexpRu)) {
          document.querySelector('.personName').classList.add('incorrect')
        }
        if(!lastName.match(regexpRu)) {
          document.querySelector('.personLastName').classList.add('incorrect')
        }
        if(!fathersName.match(regexpRu)) {
          document.querySelector('.personFathersName').classList.add('incorrect')
        }
      }
      
    },
    emailConfrim() {
      let regexp = /\\w+@\\w+.[a-z]+/;
      if(!this.formData.email.match(regexp)) {
        document.querySelector('.email__input').classList.add('incorrect')
      }
    },
    passSerConfirm() {
      if(this.formData.nationality == "Russia" || this.formData.nationality == "") {
        let regexp = /^[0-9]{4}$/;
        if(!this.formData.passSer.match(regexp)) {
          document.querySelector('.passSer').classList.add('incorrect');
        }
      }
    },
    passNumConfirm() {
      if(this.formData.nationality == "Russia") {
        let regexp = /^[0-9]{6}$/;
        if(!this.formData.passNum.match(regexp)) {
          document.querySelector('.passNum').classList.add('incorrect');
        }
      } else {
        let regexp = /^\\d+$/;
        if(!this.formData.passNum.match(regexp)) {
          document.querySelector('.passNum').classList.add('incorrect');
        }
      }
    },
    dateOfBirthConfirm() {
      if (Date.parse(this.formData.dateOfBirth) >= (new Date()).getTime() || this.formData.dateOfBirth == '') {
        document.querySelector('.dateOfBirth__input').classList.add('incorrect');
      }
    }
  },
  created: function() {
    this.throttledSearchCountry = throttle(this.getCountry, 2000)
      for(let item of citizenships) {
        this.countries.push(item.nationality);
      }
      let filteredCountries = [];
        this.countries.filter((country) => {
          if(filteredCountries.indexOf(country) == -1) {
            filteredCountries.push(country);
          }
        });
        this.countries = filteredCountries;
      for(let item of passType) {
        this.passTypes.push(item.type);
      };
      // this.debouncedCountry = debounce(this.getCountry, 2000)
   },
   watch: {
     searchWord(newValue) {
       this.throttledSearchCountry(newValue);
      // this.debouncedCountry(newValue);
     }
   },
   directives: {
    ClickOutside
  }, 
  
};
</script>

<style scoped>
.form {
 display: flex;
 flex-direction: column;
}
label {
  display: inline-block;
  margin-right: 40px;
  font-family: sans-serif;
  font-size: 24px;
}
input{
  display: block;
  margin: 10px 0;
  height: 50px;
  font-size: 24px;;
}
.name {
  width: 100%;
  display: flex;
}
input {
  border-radius: 5px;
}
.name label {
  width: 30%;
}
.name input   {
  width: 100%;
}
.dateOfBirth {
  width: 50%;
}
.dateOfBirth input {
  width: 100%;
}
.email {
  width: 100%;
}
.email__input {
  width: 100%
}
.radio {
  transform: scale(2);
}
li {
  list-style: none;
  font-size: 18px;
  margin: 2px;
}
.row .nationality-selector {
  width: 50%
}
 .nationality-selector__dropdown {
   margin: 5px 0;
   height: 100px;
   overflow-y: auto;
   width: 50%;
   border-radius: 5px;
 }
  li:hover {
   background: rgb(178, 238, 240);
   cursor: pointer;
 }
.foreigner__container {
  width: 100%;
  display: flex;
}
.foreigner__container label {
  width: 50%;
}
.foreigner__container label input{
  width: 100%
}
 .disable-scrollbars::-webkit-scrollbar {
    width: 0px;
    background: transparent;
}
.passCountry-selector li {
  width: 15%;
}
.incorrect {
  border: 1px solid red;
}
.arrow {
  position: relative;
  top: 50px;
  left: 60px
}
.arrow-nationality {
  position: relative;
  top: 50px;
  left: 85px;
}
label {
  opacity: 0.5;
}
</style>