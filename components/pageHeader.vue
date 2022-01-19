<template>
  <header>
    <h1>Добавление товара</h1>

    <button>
      <span> По умолчанию </span>

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
  </header>
</template>

<script>
export default {}
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

    span {
      font-family: Source Sans Pro;
      font-weight: 400;
      font-size: 12px;
      line-height: 15px;
    }
  }

  @media (max-width: 767.98px) {
    flex-direction: column;
    gap: 1rem;
    margin-bottom: 0.5rem;
  }
}
</style>
