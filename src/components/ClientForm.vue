<template>
  <div class="container">

    <SuccessCreate v-if="form.showModal" @close="toggleModal"/>
    <form @submit.prevent="checkValidation">
      <div class="registration">

        <div class="title">
          Регистрация
        </div>

        <label>Имя:</label>
        <input :class="$v.form.name.$error ? 'is-invalid':''" v-model.trim="$v.form.name.$model" type="text">
        <p v-if="$v.form.name.$dirty && !$v.form.name.required" class="invalid-warning">Обязательное поле</p>
        <div></div>

        <label>Фамилия:</label>
        <input v-model.trim="form.surname" type="text">
        <p v-if="$v.form.surname.$dirty && !$v.form.surname.required" class="invalid-warning">Обязательное поле</p>

        <label>Отчество:</label>
        <input type="text">

        <div class="combination">
          <div class="registration__date-number">
            <div>
              <label>Дата рождения:</label>
              <input v-model="form.dateOfBirth" type="date">
              <p v-if="$v.form.dateOfBirth.$dirty && !$v.form.dateOfBirth.required" class="invalid-warning">Укажите дату</p>
              <p v-if="$v.form.dateOfBirth.$dirty && !$v.form.dateOfBirth.minValue" class="invalid-warning">Вы из будущего о.о?</p>
            </div>
            <div>
              <label>Номер телефона:</label>
              <input @keypress="isNumber($event)" v-model.number="form.phoneNumber" type="text">
              <p v-if="$v.form.phoneNumber.$dirty && !$v.form.phoneNumber.between" class="invalid-warning">Длина номера: 11</p>
            </div>
          </div>

        </div>

        <div class="combination registration__gender-number">
          <div>
            <label>Пол:</label>
            <div class="gender">
              <label for="man">Мужской
                <input class="radio__input" type="radio" id="man" name="gender" value="man">
                <span class="checkmark"></span>
              </label>
              <label for="woman">Женский
                <input class="radio__input" type="radio" id="woman" name="gender" value="woman">
                <span class="checkmark"></span>
              </label>
            </div>
          </div>
          <div>
            <label>Группа:</label>
            <select class="group__selector" v-model="form.selectedGroups" multiple>
              <option v-for="(group, index) in form.groups" :value="group.value" :key="index">
                {{ group.label }}
              </option>
            </select>
            <p v-if="$v.form.selectedGroups.$dirty && !$v.form.selectedGroups.required" class="invalid-warning">Выберите группы</p>
          </div>
        </div>

        <div class="custom__select">
          <label>Лечащий врач:</label>
          <div class="select__arrow"></div>
          <select v-model="form.doctor">
            <option v-for="(doctor, index) in form.doctors" :key="index" :value="doctor.value">
              {{ doctor.label }}
            </option>
          </select>
        </div>

        <div class="message_accept">
          <label for="message">Не отправлять СМС</label>
          <input id="message" type="checkbox">
        </div>

      </div>
      <div class="address">
        <div class="title">
          Адрес
        </div>
        <div class="combination">
          <div>
            <div class="address__index-country">
              <div>
                <label>Индекс:</label>
                <input type="text">
              </div>
              <div>
                <label>Страна:</label>
                <input type="text">
              </div>
            </div>
          </div>
        </div>
        <div>
          <label>Область:</label>
          <input type="text">
        </div>
        <div>
          <label>Город:</label>
          <input type="text" v-model="form.city">
        </div>
        <p v-if="$v.form.city.$dirty && !$v.form.city.required" class="invalid-warning">Обязательное поле</p>
        <div class="combination">
          <div class="address__street-home">
            <div>
              <label>Улица:</label>
              <input type="text" >
            </div>
            <div>
              <label>Дом:</label>
              <input type="text">
            </div>
          </div>
        </div>
      </div>
      <div class="passport">
        <div class="title">
          Паспорт
        </div>
        <div>
          <label></label>
          <div class="custom__select">
            <div class="select__arrow"></div>
            <select v-model="form.documentType">
              <option v-for="(documentType,index) in form.documentsType" :key="index" :value="documentType.value">
                {{ documentType.label }}
              </option>
            </select>
            <p v-if="$v.form.documentType.$dirty && !$v.form.documentType.required" class="invalid-warning">Выберите документ</p>
          </div>

        </div>
        <div class="combination">
          <div class="passport__series-number">
            <div>
              <label>Серия:</label>
              <input type="text">
            </div>
            <div>
              <label>Номер:</label>
              <input type="text">
            </div>
          </div>
        </div>
        <div>
          <label>Кем выдан:</label>
          <input type="text">
        </div>
        <div>
          <label>Дата выдачи:</label>
          <input v-model="form.issueDate" type="date">
        </div>
        <p v-if="$v.form.issueDate.$dirty && !$v.form.issueDate.required" class="invalid-warning">Укажите дату</p>
        <p v-if="$v.form.issueDate.$dirty && !$v.form.issueDate.minValue" class="invalid-warning">Вы из будущего о.о?</p>
      </div>
      <input type="submit">
    </form>
  </div>
</template>

<script>
import SuccessCreate from "./SuccessCreate";
import { required, minLength, maxLength, between, numeric, minValue } from 'vuelidate/lib/validators'

export default {
  components: {SuccessCreate},
  name: "ClientForm",
  data() {
    return {
      form: {
        name: '',
        surname: '',
        dateOfBirth: '',
        phoneNumber: '7',
        selectedGroups: [],
        groups: [
          {
            label: 'VIP',
            value: 'vip'
          },
          {
            label: 'Проблемные',
            value: 'problematic'
          },
          {
            label: 'ОМС',
            value: 'oms'
          }
        ],
        doctor: '',
        doctors: [
          {
            label: 'Иванов',
            value: 'ivanov'
          },
          {
            label: 'Захаров',
            value: 'zaharov'
          },
          {
            label: 'Чернышева',
            value: 'chernesheva'
          }
        ],
        city: '',
        documentType:'',
        documentsType: [
          {
            label: 'Паспорт',
            value: 'passport'
          },
          {
            label: 'Свидетельство о рождении',
            value: 'birth_certificate'
          },
          {
            label: 'Вод. удостоверение',
            value: 'driver_license'
          }
        ],
        issueDate: '',
        showModal: false
      }

    }
  },
  validations: {
    form: {
      name: {
        required
      },
      surname: {
        required
      },
      dateOfBirth: {
        required,
        minValue: value => value < new Date().toISOString()
      },
      phoneNumber: {
        required,
        between: value => value.toString().length === 11
      },
      selectedGroups: {
        required,
        minLength: minLength(1)
      },
      city: {
        required
      },
      documentType: {
        required
      },
      issueDate: {
        required,
        minValue: value => value < new Date().toISOString()
      }
    }
  },
  methods: {
    checkValidation() {
      this.$v.form.$touch();
      if(!this.$v.form.$error) {
        this.form.showModal = !this.form.showModal;
      }
    },
    toggleModal() {
      this.form.showModal = !this.form.showModal;
    },
    isNumber: function(evt) {
      evt = (evt) ? evt : window.event;
      let charCode = (evt.which) ? evt.which : evt.keyCode;
      if ((charCode > 31 && (charCode < 48 || charCode > 57)) && charCode !== 46) {
        evt.preventDefault();
      } else {
        return true;
      }
    }
  }
}
</script>

<style lang="sass" scoped>
.container
  max-width: 560px
  margin: 0 auto

  form
    background-color: #fff
    margin: 50px 30px
    text-align: left
    padding: 40px
    border-radius: 10px
    box-shadow: 0 7px 10px #c3c3c3

    & .title
      text-align: center
      letter-spacing: 1px
      font-weight: bold
      padding-bottom: 10px

    .combination
      display: flex
      justify-content: space-between

      &div:not(:last-child)
        margin-right: 20px

      & input
        height: 40px

      & .registration__date-number
        display: grid
        grid-template-columns: 1fr 1fr
        grid-column-gap: 3vw

      & .group__selector
        height: auto
        background-color: #e2e2e2
        width: 150px
        border-radius: 10px
        margin-top: 10px

      & .address__index-country
        display: grid
        grid-template-columns: .5fr 1.5fr
        grid-column-gap: 3vw

      & .address__street-home
        display: grid
        grid-template-columns: 1.5fr .5fr
        grid-column-gap: 3vw

      & .passport__series-number
        display: grid
        grid-template-columns: 1.3fr .7fr
        grid-column-gap: 3vw

      & .registration__date-number
        @media (max-width: 490px)
          display: block
          width: 100%

      &.registration__gender-number
        @media (max-width: 490px)
          display: block

          & select
            width: 100%

    & .registration
      border-bottom: 1px solid
      padding-bottom: 20px

      & .gender
        display: flex

        & label
          display: inline-flex
          margin-right: 10px
          color: #404040

          & .radio__input
            display: none

          & .checkmark
            width: .8rem
            height: .8rem
            border-radius: 50%
            border: 2px solid teal
            margin-left: 10px
            padding: 2px

          & .checkmark::after
            content: ''
            display: none
            width: 100%
            height: 100%
            border-radius: 50%
            background-color: teal

          & .radio__input:checked + .checkmark::after
            display: block

      & .message_accept
        display: flex
        margin-top: 40px
        justify-content: center

        & label
          margin: 0
          color: black

        & input
          justify-content: left
          text-align: left
          float: left
          width: 40px

    & .address

      & .title
        margin: 20px 0 15px

    & .passport
      margin-top: 40px
      padding-bottom: 10px

    label
      color: #aaa
      display: block
      font-size: .7rem
      text-transform: uppercase
      letter-spacing: 1px
      font-weight: bold
      margin: 20px 0 0

    input
      background-color: white
      transition: 1s
      display: block
      padding: 7px 6px
      font-size: .9rem
      width: 100%
      border: none
      border-bottom: 1px solid #ddd
      color: #616161
      font-weight: bold

    input:focus
      border-bottom: 0 black solid
      transition: 1s
      border-bottom: 1px black solid

    select
      background-color: white
      padding: 7px 6px
      font-size: .9rem
      width: 100%
      border: none
      border-bottom: 1px solid #ddd
      color: #454545
      font-weight: bold
      -webkit-appearance: none
      -moz-appearance: none
      text-indent: 1px
      text-overflow: ''

    .select__arrow:before
      content: '\1F783'
      position: absolute
      pointer-events: none

    .custom__select

      & .select__arrow
        text-align: right
        margin-right: 15px

    .invalid-warning
      font-size: 12px
      color: red
      font-weight: 800
      margin-top: 8px

    input[type=submit]
      transition: .2s
      width: 100px
      height: 40px
      margin: 15px auto 0 auto
      border: 2px solid blue
      background-color: blue
      border-radius: 20px
      color: #fff

    input[type=submit]:hover
      transition: .2s
      background-color: #0404e0

    input[type=submit]:active
      transition: .2s
      background-color: teal

</style>