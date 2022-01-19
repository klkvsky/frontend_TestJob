<template>
  <main>
    <InputForm @addItem="addItemToGrid" />
    <transition-group name="list" class="productGrid">
      <!-- Передаем компоненту Card данные в виде пропа из cards, для рендеринга информации -->
      <Card
        @removeItemFrom="removeItemFromGrid(index)"
        :card="card"
        v-for="(card, index) in sortFunc"
        :key="card.id"
      />
    </transition-group>
  </main>
</template>

<script>
import InputForm from '~/components/inputForm.vue' // Импорт формы добавления товара
import Card from '~/components/card.vue' // Импорт компонента карточки

export default {
  layout: 'default',
  components: {
    InputForm,
    Card,
  },
  props: ['sortingValue'], // Проп получаемый из layouts/default.vue с данными о типе сортировки
  data() {
    return {
      cards: [
        // Массив карточек-плейсхолдеров, куда так же добавляются новые карточки
        {
          imageURL: '/_nuxt/assets/image.png',
          title: 'Наименование товара 1',
          description:
            'Довольно-таки интересное описание товара в несколько строк.Довольно-таки интересное описание товара в несколько строк',
          price: '10 000',
          id: 1,
        },
        {
          imageURL: '/_nuxt/assets/image.png',
          title: 'Наименование товара 2',
          description:
            'Довольно-таки интересное описание товара в несколько строк.Довольно-таки интересное описание товара в несколько строк',
          price: '10 000',
          id: 2,
        },
        {
          imageURL: '/_nuxt/assets/image.png',
          title: 'Наименование товара 3',
          description:
            'Довольно-таки интересное описание товара в несколько строк.Довольно-таки интересное описание товара в несколько строк',
          price: '10 000',
          id: 3,
        },
        {
          imageURL: '/_nuxt/assets/image.png',
          title: 'Наименование товара 4',
          description:
            'Довольно-таки интересное описание товара в несколько строк.Довольно-таки интересное описание товара в несколько строк',
          price: '10 000',
          id: 4,
        },
        {
          imageURL: '/_nuxt/assets/image.png',
          title: 'Наименование товара 5',
          description:
            'Довольно-таки интересное описание товара в несколько строк.Довольно-таки интересное описание товара в несколько строк',
          price: '5 000',
          id: 5,
        },
        {
          imageURL: '/_nuxt/assets/image.png',
          title: 'Наименование товара 6',
          description:
            'Довольно-таки интересное описание товара в несколько строк.Довольно-таки интересное описание товара в несколько строк',
          price: '8 000',
          id: 6,
        },
        {
          imageURL: '/_nuxt/assets/image.png',
          title: 'Наименование товара 7',
          description:
            'Довольно-таки интересное описание товара в несколько строк.Довольно-таки интересное описание товара в несколько строк',
          price: '9 000',
          id: 7,
        },
        {
          imageURL: '/_nuxt/assets/image.png',
          title: 'Наименование товара 8',
          description:
            'Довольно-таки интересное описание товара в несколько строк.Довольно-таки интересное описание товара в несколько строк',
          price: '12 000',
          id: 8,
        },
        {
          imageURL: '/_nuxt/assets/image.png',
          title: 'Наименование товара 9',
          description:
            'Довольно-таки интересное описание товара в несколько строк.Довольно-таки интересное описание товара в несколько строк',
          price: '11 000',
          id: 9,
        },
      ],
      id: 9,
    }
  },
  mounted() {
    if (localStorage.cards) {
      this.cards = JSON.parse(localStorage.getItem('cards'))
    } // Из localStorage получаем список сохранившихся карточек
    if (localStorage.id) {
      this.id = JSON.parse(localStorage.getItem('id'))
    } // Из localStorage получаем сохранившийся id, чтобы не произошла дубликация :key элемента при v-for
  },
  computed: {
    /* Функция сортировки карточек с пропом sortingValue */
    sortFunc() {
      if (this.sortingValue === 'По цене min') {
        this.cards.sort((x, y) => {
          return parseInt(x.price) > parseInt(y.price)
        })
        return this.cards
      } else if (this.sortingValue === 'По цене max') {
        this.cards.sort((x, y) => {
          return parseInt(x.price) < parseInt(y.price)
        })
        return this.cards
      } else if (this.sortingValue === 'По наименованию') {
        this.cards.sort((x, y) => {
          return x.title > y.title ? 1 : -1
        })
        return this.cards
      } else {
        return this.cards
      }
    },
  },
  methods: {
    /* Функция добавления карточки в массив cards, данные получаем из components/inputForm.vue */
    addItemToGrid(value) {
      this.cards.unshift({
        imageURL: value.PassedURL,
        title: value.PassedTitle,
        description: value.PassedDesctiption,
        price: value.PassedPrice.toLocaleString('ru-RU'), // Переводим цену в локальную систему разрядов
        id: this.id + 1,
      })
      this.id++ // Увеличиваем id для того, чтобы не дублировался :key параметр в v-for
      this.setLocalStorage() // Заносим данные в localStorage для сохранения списка при перезагрузке
    },
    setLocalStorage() {
      localStorage.setItem('cards', JSON.stringify(this.cards))
      localStorage.setItem('id', JSON.stringify(this.id))
    },
    /* Функция удаления карточки из массива cards, получает emit из компенента карточки, индекс берем из v-for */
    removeItemFromGrid(index) {
      this.cards.splice(index, 1)
      this.setLocalStorage() // Заносим данные в localStorage для сохранения списка при перезагрузке
    },
  },
}
</script>

<style scoped lang="scss">
main {
  display: flex;
  flex-direction: row;
  align-items: flex-start;
  justify-content: flex-start;
  gap: 1rem;

  .productGrid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    flex-wrap: wrap;
    gap: 1rem;
    margin-bottom: 3rem;
  }

  @media (max-width: 767.98px) {
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 3rem;

    .productGrid {
      grid-template-columns: 1fr;
      justify-items: center;
      width: 100%;
    }
  }

  .list-enter-active,
  .list-leave-active {
    transition: all 1s ease-in-out;
  }
  .list-enter {
    opacity: 0;
    transform: translateX(-30px);
    transition: 1s all ease-in-out;
  }

  .list-leave-to {
    opacity: 0;
    animation: card__delete 1.5s infinite;
    transition: 1.5s all ease-in-out;
  }

  @keyframes card__delete {
    0% {
      transform: translateY(0);
    }
    100% {
      transform: translateY(-200px);
    }
  }

  @media (max-width: 1200px) {
    // Медиа query для маленьких мониторов / Планшетов
    .productGrid {
      grid-template-columns: repeat(2, 1fr);
    }
  }

  @media (max-width: 1024px) {
    // Медиа query для старых мониторов / Планшетов с маленькой диагональю
    .productGrid {
      grid-template-columns: repeat(1, 1fr);
    }
  }
}
</style>
