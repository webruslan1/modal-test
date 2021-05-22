<template>
  <div class="modal-backdrop" @click="outSide($event)">
    <div class="modal">
      <button type="button" class="btn-close" @click="close"></button>
      <header class="modal-header">
        <h1 class="modal-title">Налоговый вычет</h1>
        <p class="modal-text">
          Используйте налоговый вычет чтобы погасить ипотеку досрочно. <br />
          Размер налогового вычета составляет не более 13% от своего
          официального годового дохода.
        </p>
      </header>

      <main class="modal-main">
        <h2 class="input-title">Ваша зарплата в месяц</h2>
        <input
          :class="{ 'input-error': inputValueCount }"
          v-model="mainInput"
          @input="inputValue(mainInput)"
          class="main-input"
          type="text"
          placeholder="Введите данные"
        />
        <p v-if="inputValueCount" class="main-input__error">
          Поле обязательно для заполнения
        </p>
        <button class="input-btn" @click="takeValue">Рассчитать</button>
      </main>

      <div v-if="valueHave" class="modal-list">
        <h2>Итого можете внести в качестве досрочных:</h2>
        <ul class="checkbox-list">
          <li
            class="checkbox-list__item"
            v-for="(item, id) in valueHave"
            :key="id"
          >
            <div class="checkbox_btn">
              <input :id="'checkbox-' + id" type="checkbox" />
              <label :for="'checkbox-' + id">
                <img src="@/assets/galka.svg" alt="" />
              </label>
            </div>
            <span
              ><strong>{{ item }} рублей</strong> в {{ id + 1 }}-ый год</span
            >
          </li>
        </ul>
      </div>

      <div class="modal-footer">
        <div class="choice">
          <h2 class="choice-title">Что уменьшаем?</h2>
          <div class="choice-list">
            <button @click="activeBtn = true"  :class="{'active-btn': activeBtn}"  class="choice-list__button">Платёж</button>
            <button @click="activeBtn = false" :class="{'active-btn': !activeBtn}" class="choice-list__button">Срок</button>
          </div>
        </div>
        <button class="choice-btn">Добавить</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Modal",
  methods: {
    close() {
      this.$emit("close");
    },
    formattedPrice(value) {
      if (value) {
        let parts = value.toString().split(".");
        parts[0] = parts[0].replace(/\B(?=(\d{3})+(?!\d))/g, " ");
        return parts.join(".");
      } else {
        return value;
      }
    },
    inputValue(val) {
      if (val) {
        var someVal = this.formattedPrice(val.replace(/\s/g, ''))
        this.mainInput = someVal
      } else {
        return val;
      }
    },
    takeValue() {
      var inputValue = parseInt(document.querySelector(".main-input").value.replace(/\s/g, ''));

      if (inputValue > 0) {
        this.inputValueCount = false;
        this.valueHave = [];
        var threeYears = inputValue * 12 * 0.13;
        var fourYear = (inputValue * 12 * 0.13) / 3;
        this.valueHave.push(this.formattedPrice(Math.floor(threeYears)));
        this.valueHave.push(this.formattedPrice(Math.floor(threeYears)));
        this.valueHave.push(this.formattedPrice(Math.floor(threeYears)));
        this.valueHave.push(this.formattedPrice(Math.floor(fourYear)));
        console.log(this.valueHave);
      } else {
        this.valueHave = null,
        this.inputValueCount = true;
      }
    },
    outSide(event){
      var modal = document.querySelector('.modal')
      if(!modal.contains(event.target)){
        this.close();
      }
    },
  },
  data() {
    return {
      mainInput: null,
      inputValueCount: false,
      valueHave: null,
      activeBtn: true
    };
  },
  mounted(){
    var pText = document.querySelector('.modal-text')
    const screenWidth = window.screen.width
    if(screenWidth < 450){
       pText.innerHTML = pText.innerHTML.replace('<br>', '');
    }
    console.log(screenWidth)
  }
};
</script>

<style>


input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type="number"] {
  -moz-appearance: textfield;
}
.main-input__error {
  font-size: 10px;
  line-height: 12px;
  color: #ea0029;
  margin: 0;
}

.choice-btn {
  background: linear-gradient(
      255.35deg,
      #dc3131 0.83%,
      rgba(255, 79, 79, 0) 108.93%
    ),
    #ff5e56;
  box-shadow: 0px 0px 24px rgba(234, 0, 41, 0.33);
  border-radius: 6px;
  width: 100%;
  font-weight: 500;
  font-size: 16px;
  line-height: 24px;
  color: #ffffff;
  border: none;
  cursor: pointer;
  padding: 16px 0;
  transition: 300ms;
}

.choice-btn:hover {
  background: #ea0029;
  box-shadow: 0px 0px 24px rgba(234, 0, 41, 0.33);
  color: #fff;
  transition: 300ms;
}

.choice-btn:disabled {
  background: #bec5cc;
}
.choice {
  display: flex;
  align-items: center;
  margin-bottom: 40px;
}

.choice-list__button {
  background: #eef0f2;
  border-radius: 50px;
  border: none;
  padding: 6px 12px;
  font-size: 14px;
  line-height: 24px;
  cursor: pointer;
}

.choice-list__button:hover {
  background: #dfe3e6;
}

.choice-list__button.active-btn {
  background: linear-gradient(
      255.35deg,
      #dc3131 0.83%,
      rgba(255, 79, 79, 0) 108.93%
    ),
    #ff5e56;
  color: #fff;
}

.choice-list__button:not(:last-child) {
  margin-right: 16px;
}

.choice-title {
  margin-right: 32px;
}

.modal-main {
  margin-bottom: 16px;
}

.checkbox-list {
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  list-style-type: none;
}

.modal-list {
  margin-bottom: 12px;
}

.modal-footer {
  padding-top: 8px;
}

.checkbox-list__item {
  display: flex;
  align-items: center;
  padding-bottom: 16px;
  padding-top: 16px;
  border-bottom: 1px solid #dfe3e6;
}

.checkbox-list__item span {
  font-size: 14px;
  line-height: 24px;
  color: #808080;
}
.checkbox-list__item span strong {
  font-weight: normal;
  color: #000;
}
.checkbox_btn {
  display: inline-block;
  margin-right: 10px;
}
.checkbox_btn input[type="checkbox"] {
  display: none;
}
.checkbox_btn label {
  margin-right: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  border-radius: 6px;
  user-select: none;
  background: #ffffff;
  border: 1px solid #dfe3e6;
  box-sizing: border-box;
  border-radius: 6px;
  width: 20px;
  height: 20px;
}

.checkbox_btn input[type="checkbox"]:checked + label {
  background: linear-gradient(
      255.35deg,
      #dc3131 0.83%,
      rgba(255, 79, 79, 0) 108.93%
    ),
    #ff5e56;
  border-color: #fff;
}

.checkbox_btn input[type="checkbox"]:disabled + label {
  background: #bec5cc;
}

.checkbox_btn input[type="checkbox"] + label:hover {
  border-color: #000;
}

.modal-backdrop {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
}

h2 {
  font-weight: 500;
  font-size: 14px;
  line-height: 24px;
  color: #000000;
  padding: 0;
  margin: 0;
}

.main-input {
  height: 40px;
  border: 1px solid #dfe3e6;
  border-radius: 3px;
  box-sizing: border-box;
  padding: 8px 10px;
  width: 100%;
  font-family: "Lab Grotesque", sans-serif;
  font-size: 14px;
  line-height: 24px;
  color: #000;
  margin-bottom: 4px;
}

.main-input.input-error {
  border: 1px solid #ea0029 !important;
}

.main-input:hover {
  border: 1px solid #000;
}

.main-input:focus {
  outline: none;
}

.main-input:disabled {
  border: 1px solid #808080;
}

.main-input::placeholder {
  font-family: "Lab Grotesque", sans-serif;
  font-size: 14px;
  line-height: 24px;
  color: #bec5cc;
}

.main-input:disabled::placeholder {
  color: #808080;
}

.input-title {
  margin-bottom: 8px;
}

.input-btn {
  font-weight: 500;
  font-size: 14px;
  line-height: 24px;
  color: #ea0029;
  background: transparent;
  border: none;
  padding: 0;
  cursor: pointer;
  margin-top: 8px;
}

.input-btn:hover {
  color: #f53a31;
}

.modal {
  background: #ffffff;
  overflow-x: auto;
  flex-direction: column;
  padding: 32px;
  width: 552px;
  position: relative;
  border-radius: 30px;
  box-sizing: border-box;
}

.btn-close {
  position: absolute;
  top: 27px;
  right: 27px;
  border: none;
  cursor: pointer;
  color: #ea0029;
  background: transparent;
}

.btn-close:hover {
  opacity: 0.6;
}

.btn-close::before,
.btn-close::after {
  content: "";
  display: block;
  position: absolute;
  width: 3px;
  height: 18px;
  background: #ea0029;
  border-radius: 30px;
}

.btn-close::before {
  transform: rotate(-45deg);
}
.btn-close::after {
  transform: rotate(45deg);
}

.modal-title {
  font-weight: 500;
  font-size: 28px;
  line-height: 40px;
  color: #000000;
  padding: 0;
  margin: 0;
  margin-bottom: 16px;
}

.modal-text {
  font-size: 14px;
  line-height: 24px;
  color: #808080;
}

@media(max-width:769px){
  .modal{
    width: 453px;
  }
}

@media(max-width:450px){
  .btn-close{
    top: 22px;
    right: 22px;
  }
  .modal{
    width: 100vw;
    height: 100vh;
    padding: 32px 16px 16px 16px;
    border-radius: 0;
  }

  .modal-title{
    font-size: 18px;
    line-height: 24px;
  }

  .modal-text{
    font-size: 12px;
    line-height: 16px;
  }
  
  h2{
    font-size: 14px;
    line-height: 24px;
  }

  .modal-list h2{
    width: 150px;
  }

  .choice{
    flex-direction: column;
    justify-content: center;
    align-items: unset;
    margin-bottom: 100px;
  }

  .choice-title{
    margin-bottom: 24px;
  }

  .choice-btn{
    position: absolute;
    bottom: 16px;
    left: 16px;
    width: 90%;
  }
}
</style>