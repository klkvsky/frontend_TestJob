<template>
  <header>
    <h1>Добавление товара</h1>

    <div class="selectSorting" @click="dropdown">
      <button>
        <span> {{ CurrentSelection }} </span>

        <svg
          width="8"
          height="6"
          viewBox="0 0 8 6"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path
            d="M7.48532 1.24264L4.24268 4.48528L1.00003 1.24264"
            stroke="#B4B4B4"
          />
        </svg>
      </button>

      <div class="selectSorting__options" :class="{ active: isDropdown }">
        <span @click="CurrentSelection='По цене min'"> По цене min </span>
        <span @click="CurrentSelection='По цене max'"> По цене max </span>
        <span @click="CurrentSelection='По наименованию'"> По наименованию </span>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  data() {
    return {
      CurrentSelection: 'По умолчанию',
      isDropdown: false,
    }
  },
  watch: {
    CurrentSelection(){
      this.$emit('changeSorting', this.CurrentSelection)
    }
  },
  methods: {
    dropdown() {
      this.isDropdown = !this.isDropdown
    },
  },
}
</script>

<style scoped lang="scss">
header {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  margin-top: 2rem;
  margin-bottom: 1rem;

  h1 {
    font-family: Source Sans Pro;
    font-weight: 600;
    font-size: 28px;
    line-height: 35px;
    color: #3f3f3f;
  }

  .selectSorting {
    position: relative;
    width: 150px;

    &__options {
      position: absolute;
      top: 41px;
      left: 0;
      z-index: -100;
      display: flex;
      flex-direction: column;
      align-items: center;
      text-align: center;
      border: none;
      background: #fffefb;
      box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
      border-radius: 4px;

      width: 150px;

      transition: 0.2s all ease-in-out;
      transform: translateY(-41px);
      opacity: 0;

      &.active {
        transform: translateY(0);
        opacity: 1;
        z-index: 100;
        transition: 0.2s all ease-in-out;
      }

      span {
        font-family: Source Sans Pro;
        font-weight: 400;
        font-size: 12px;
        line-height: 15px;
        padding: 0.5rem 1rem;

        width: 100%;
        white-space: nowrap;
        cursor: pointer;

        &:first-child {
          border-top-left-radius: 4px;
          border-top-right-radius: 4px;
        }

        &:last-child {
          border-bottom-left-radius: 4px;
          border-bottom-right-radius: 4px;
        }

        // transition: 0.2s all ease-in-out;
        &:hover {
          background-color: #4bb543;
          color: #ffffff;
          transition: 0.05s all ease-in-out;
        }
      }
    }

    button {
      display: flex;
      flex-direction: row;
      justify-content: space-between;
      align-items: center;
      gap: 0.5rem;
      padding: 1rem;
      height: 36px;
      border: none;
      background: #fffefb;
      box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
      border-radius: 4px;
      cursor: pointer;

      width: 150px;

      span {
        font-family: Source Sans Pro;
        font-weight: 400;
        font-size: 12px;
        line-height: 15px;
      }
    }
  }

  @media (max-width: 767.98px) {
    flex-direction: column;
    gap: 1rem;
    margin-bottom: 0.5rem;
  }
}
</style>
