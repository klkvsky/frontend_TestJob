<template>
  <!-- Оборачиваем прелоадер в transition для облегчения работы с анимациями  -->
  <transition name="fade">
    <!-- Прелоадер активен, если show === true -->
    <div v-if="show" class="preloader">
      <h1>🐣</h1>
      <h1>Добро пожаловать</h1>
    </div>
  </transition>
</template>

<script>
export default {
  data() {
    return {
      show: true, // Отвечает за состояние прелоадера, при false прелоадер уходит
    }
  },
  mounted() {
    this.activePreloader()
    // При загрузке активируем activePreloader(), для посыла сигнала о начале закрывания прелоадера
  },
  methods: {
    // Метод через 1.5 секунды удаляет прелоадер, по свойствам назначения show обьекта false значения
    activePreloader() {
      setTimeout(() => {
        this.show = false
      }, 1500)
    },
  },
}
</script>

<style scoped lang="scss">
.preloader {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: #fff;
  z-index: 9999;

  h1 {
    font-family: Source Sans Pro;
    font-weight: 600;
    font-size: 28px;
    line-height: 35px;
    color: #3f3f3f;

    &:first-child {
      font-size: 6rem;
      margin-bottom: 3rem;
    }
  }
}

.fade-enter-acitve,
.fade-leave-active {
  transition: 1s opacity;

  h1 {
    animation: popOut 1.5s linear;
  }
}

.fade-enter,
.fade-leave-to {
  opacity: 0;

  h1 {
    animation: popOut 1.5s linear;
  }
}

// Анимация ухода элементов прелоадера
@keyframes popOut {
  0% {
    transform: scale(1);
  }
  100% {
    transform: scale(0);
  }
}
</style>
