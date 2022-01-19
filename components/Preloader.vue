<template>
  <transition name="fade">
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
            show: true
        }
    },
    mounted() {
        this.activePreloader()
    },
    methods: {
        activePreloader(){
            setTimeout(() => {
                this.show = false
            }, 1500);
        }
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

.fade-enter, .fade-leave-to{
    opacity: 0;
}

@keyframes popOut {
    0% { transform: scale(1);}
    100% { transform: scale(0);}
}

@keyframes popIn {
    0% { transform: scale(0);}
    100% { transform: scale(1);}
}
</style>
