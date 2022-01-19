<template>
  <div class="inputForm">
    <!-- Активируем класс success при удачном добавлении товара -->
    <div class="inputForm__success" :class="{ success: success }">
      <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
        <path
          d="m10 15.586-3.293-3.293-1.414 1.414L10 18.414l9.707-9.707-1.414-1.414z"
        ></path>
      </svg>
    </div>

    <!-- Функционал добавления карточки при нажатии кнопки ENTER, во время ввода инпута внутри формы -->
    <form @keydown.enter="addItem()">
      <label class="required" for="objectName"> Наименование товара </label>

      <!-- Каждый input в данной форме при активации запускает validation метод, для того чтобы при ошибке с пустыми полями, сообщение об ошибке и красная обводка исчезали при занесении хотя бы одного элемента в input -->
      <input
        v-model="title"
        type="text"
        @input="validation"
        :class="{ danger: titleEmpty }"
        required
        placeholder="Введите наименование товара"
      />

      <p v-if="titleEmpty">Поле является обязательным</p>
      <!-- Сообщение об ошибке, при попытке добавления с пустым кодом, так же от titleEmpty зависит класс input элемента, который при активации добавляет класс danger c красной обводкой-->

      <label for="objectDescription"> Описание товара </label>
      <textarea
        v-model="description"
        name="objectDescription"
        id="objectDescription"
        cols="30"
        rows="6"
        placeholder="Введите описание товара"
      >
      </textarea>

      <label class="required" for="objectImage">
        Ссылка на изображение товара
      </label>
      <input
        v-model="imageURL"
        type="url"
        @input="validation"
        :class="{ danger: imageURLEmpty }"
        name="objectImage"
        required
        placeholder="Введите ссылку"
      />
      <p v-if="imageURLEmpty">Поле является обязательным</p>

      <label class="required" for="objectPrice"> Цена товара </label>

      <!-- Присваем значение для input'а validPrice, так как v-model не позволяет делать маску ввода -->
      <!-- При внесении информации в инпут, запускаем функцию onChange, которая конвертирует string в int и присваивает int значение другому обьекту newPrice -->
      <input
        :value="validPrice"
        @input="validation(), onChange($event)"
        :class="{ danger: priceEmpty }"
        type="text"
        name="objectPrice"
        required
        placeholder="Введите цену"
      />
      <p v-if="priceEmpty">Поле является обязательным</p>

      <!-- При пустых required полях активен класс disabled и при нажатии в методе addItem сначала проверяется пусты ли поля и если пусты показываются ошибки  -->
      <button
        :class="{ disabled: isDisabled }"
        type="button"
        @click="addItem()"
      >
        Добавить товар
      </button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: '',
      description: '',
      imageURL: '',
      price: '',
      newPrice: 0, // Обьект которому присваевается int значение price при вводе цены

      /* Список данных отвечающих за проверку состояния required инпутов */
      titleEmpty: false,
      imageURLEmpty: false,
      priceEmpty: false,
      /* /Конец списка/ */

      isDisabled: true, // Обьект отвечающий за активность / неактивность кнопки добавления

      success: false, // Обьект === true первые 2 секунды после удачного добавления карточки, отвечает за анимацию удачной отправки
    }
  },
  watch: {
    // Список watch'еров для проверки required заполнения полей, при пустом поле, активирует соответсвенный обьект в data()
    title() {
      if (this.title) {
        this.titleEmpty = false
      }
    },
    imageURL() {
      if (this.imageURL) {
        this.imageURLEmpty = false
      }
    },
    price() {
      if (this.price) {
        this.priceEmpty = false
      }
    },
  },
  computed: {
    // Функция для перевода int значения newPrice в локальную систему разрядов, при пустом значении newPrice ничего не выводит
    validPrice() {
      if (!this.newPrice) {
        return ''
      } else {
        return this.newPrice.toLocaleString('ru-RU')
      }
    },
  },
  methods: {
    // Метод для одновременной проверки сразу 3 required полей, при заполнении всех 3, меняет isDisabled на false и активирует кнопку добавления
    validation() {
      if (this.title && this.imageURL && this.price) {
        this.isDisabled = false
        return false
      } else {
        this.isDisabled = true
        return true
      }
    },
    // Метод для удаления пробелов из данных input цены и присваивания этих данных newPrice в виде int
    onChange($) {
      this.price = $.target.value.replace(/\D/g, '')
      this.newPrice = parseInt(this.price)
    },
    // Метод добавления товара из формы, проверяет заполнены ли required поля, при успехе, посылат emit (addItem) parent элементу (index.vue) с обьектом для новой карточки, запускает метод очистки формы, запускает метод активации анимации успешного добавления
    // Если формы не заполнены, то активирует checkInput, для проверки какие именно поля не заполнены
    addItem() {
      if (this.isDisabled == false) {
        this.$emit('addItem', {
          PassedTitle: this.title,
          PassedDesctiption: this.description,
          PassedURL: this.imageURL,
          PassedPrice: (this.price = parseInt(this.price)),
        }),
          this.resetForm()
        this.animateSuccess()
      } else {
        this.checkInput()
      }
    },
    // Метод активации класса успешного добавления, заканчивает анимацию через 2 секунды
    animateSuccess() {
      this.success = true

      setTimeout(() => {
        this.success = false
      }, 2000)
    },
    // Мето для проверки какие именно required поля не заполнены
    checkInput() {
      if (!this.title) {
        this.titleEmpty = true
      } else {
        this.titleEmpty = false
      }

      if (!this.imageURL) {
        this.imageURLEmpty = true
      } else {
        this.imageURLEmpty = false
      }

      if (!this.price) {
        this.priceEmpty = true
      } else {
        this.priceEmpty = false
      }
    },
    // Метод для сбрасывания формы при удачном добавлении товара
    resetForm() {
      this.title = ''
      this.description = ''
      this.imageURL = ''
      this.price = ''
      this.newPrice = ''
      this.isDisabled = true
      this.titleEmpty = false
      this.imageURLEmpty = false
      this.priceEmpty = false
    },
  },
}
</script>

<style scoped lang="scss">
.inputForm {
  background: #fffefb;
  box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04),
    0px 6px 10px rgba(0, 0, 0, 0.02);
  border-radius: 4px;
  width: 332px;

  padding: 1.5rem;

  position: sticky;
  top: 1rem;

  overflow: hidden;

  // Стили для карточки успешного добавления
  &__success {
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    left: 0;
    background-color: #4bb543;
    border-radius: 4px;

    display: flex;
    align-items: center;
    justify-content: center;

    transform: scaleY(0);
    transform-origin: bottom;
    transition: transform 0.5s ease-in-out;

    // При активации класса, запускается анимация успешного добавления
    &.success {
      opacity: 1;
      transition: transform 0.8s ease-in-out;
      transform: scaleY(1);
      transform-origin: top;
      z-index: 100;

      svg {
        align-self: center !important;
      }
    }

    svg {
      fill: #ffffff;
      min-width: 4rem;
      min-height: 4rem;
      align-self: center !important;
    }
  }

  form {
    display: flex;
    flex-direction: column;

    label {
      display: flex;
      flex-direction: row;
      align-items: center;
      margin-top: 1rem;
      margin-bottom: 0.2rem;

      &:first-child {
        margin-top: 0;
      }
      font-family: Source Sans Pro;
      font-weight: 400l;
      font-size: 10px;
      line-height: 13px;
      letter-spacing: -0.02em;

      color: #49485e;

      &.required:after {
        content: ' ';
        background-color: #ff8484;
        width: 4px;
        height: 4px;
        border-radius: 100px;
        display: block;
        margin-top: -0.65rem;
        margin-left: 0.05rem;
      }
    }

    input,
    textarea {
      border: 1px solid transparent;
      background: #fffefb;
      box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
      border-radius: 4px;
      padding: 0.7rem 1rem;

      font-family: Source Sans Pro;
      font-weight: 400;
      font-size: 12px;
      line-height: 15px;
      color: #49485e;

      resize: none;
      transition: 0.2s all ease-in-out;

      &::placeholder {
        color: #b4b4b4;
      }

      &:focus {
        outline: none;
        border: 1px solid #333;
        transition: 0.2s all ease-in-out;
      }

      &.danger {
        border: 1px solid #ff8484;
      }
    }

    textarea {
      padding: 0.6rem 1rem;
    }

    p {
      font-family: Source Sans Pro;
      font-weight: 400;
      font-size: 0.7rem;
      margin-top: 0.5rem;
      line-height: 10px;
      letter-spacing: -0.02em;
      color: #ff8484;
    }

    button {
      border-radius: 10px;
      border: none;
      padding: 0.65rem;
      box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
      margin-top: 1.5rem;
      width: 100%;

      font-family: Inter;
      font-weight: 600;
      font-size: 12px;
      line-height: 15px;
      text-align: center;
      letter-spacing: -0.02em;

      background: #7bae73;
      color: #ffffff;

      cursor: pointer;
      transition: 0.2s all ease-in-out;

      &:hover {
        opacity: 0.7;
        transition: 0.2s all ease-in-out;
      }

      &:active {
        transform: scale(0.95);
        transition: 0.2s all ease-in-out;
      }

      &.disabled {
        background: #eeeeee;
        color: #b4b4b4;
      }
    }
  }

  // Стили для мобильных устройств
  @media (max-width: 767.98px) {
    position: relative;
    width: 95%;
  }
}
</style>
