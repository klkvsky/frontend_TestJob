<template>
  <div class="container">
    <div class="container__child">
      <Preloader />

      <!-- changeSorting получает данные из компонента шапки -> селектора сортировки о типе сортировки -->
      <PageHeader @changeSorting="changeSorting" />
      <!-- Передаем данные о типе сортировки в виде пропа <nuxtChild /> компоненту-->
      <NuxtChild :sortingValue="this.sortingValue" />
      <!-- Пришлось поменять <Nuxt />, так как нельзя передавать пропы данному обьекту, но  <NuxtChild /> исполняет ту же роль и имеет возможность принимать пропы -->
      <FooterComponent />
    </div>
  </div>
</template>

<script>
import PageHeader from '~/components/pageHeader.vue'
import Preloader from '~/components/Preloader.vue'
import FooterComponent from '~/components/FooterComponent.vue'

export default {
  data() {
    return {
      sortingValue: 'По умолчанию',
    }
  },
  components: {
    PageHeader,
    Preloader,
    FooterComponent,
  },
  methods: {
    /* При запуске функции присваевается значение из селектора в виде value data компоненту sortingValue, который передает эти данные в виде пропа <NuxtChild /> */
    changeSorting(value) {
      this.sortingValue = value
    },
  },
}
</script>

<style scoped lang="scss">
.container {
  padding: 0 2.2%;
  background-color: rgba(255, 254, 251, 0.8);
  height: 100vh;
  display: flex;
  justify-content: center;

  // Добавил второй контейнер, для центровки div'а на больших мониторах
  &__child {
    display: flex;
    flex-direction: column;
    max-width: 1400px;
  }
}
</style>

<!-- Сделал этот блок не scoped, потому что эти пораметри обязательно считаю нужным указать при верстке, для избежания проблем, но v-deep: и аналоги не дают такого функционала -->
<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
