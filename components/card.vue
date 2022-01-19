<template>
  <div class="card">
    <div class="card__img">
      <img :src="card.imageURL" />
    </div>
    <div class="card__body">
      <h3>{{ card.title }}</h3>
      <p>
        {{ card.description }}
      </p>
      <h2>{{ card.price }} руб.</h2>
    </div>

    <DeleteButton @removeItem="removeItemFrom"/>
  </div>
</template>

<script>
import DeleteButton from '~/components/deleteButton.vue'

export default {
  components: {
    DeleteButton,
  },
  props: ['card'],
  methods: {
    removeItemFrom(){
      this.$emit('removeItemFrom')
    }
  },
}
</script>

<style scoped lang="scss">
.card {
  background: #fffefb;
  box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04),
    0px 6px 10px rgba(0, 0, 0, 0.02);
  border-radius: 4px;
  width: 332px;
  height: 423px;
  position: relative;

  transition: all 1s ease-in-out;

  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: 47% 53%;

  &:hover {
    cursor: pointer;
    .card__delete {
      opacity: 1;
      transition: 0.2s all ease-in-out;
    }
  }

  &__img {
    overflow: hidden;
    img {
      object-fit: cover;
      width: 100%;
      height: 100%;
      border-top-left-radius: 4px;
      border-top-right-radius: 4px;
    }
  }

  &__body {
    padding: 1rem;
    padding-bottom: 1.5rem;
    display: grid;
    gap: 1rem;
    grid-template-columns: 1fr;
    grid-template-rows: 1fr 3fr 1fr;

    h3 {
      font-family: Source Sans Pro;
      font-style: normal;
      font-weight: 600;
      font-size: 20px;
      line-height: 25px;

      color: #3f3f3f;
    }

    p {
      font-family: Source Sans Pro;
      font-style: normal;
      font-weight: normal;
      font-size: 16px;
      line-height: 20px;

      color: #3f3f3f;
    }

    h2 {
      font-family: Source Sans Pro;
      font-style: normal;
      font-weight: 600;
      font-size: 24px;
      line-height: 30px;

      color: #3f3f3f;
    }
  }


  @media (max-width: 767.98px){
  	width: 95%;
    height: 350px;

    &__body{
      padding: 6vw;
      padding-bottom: 6.5vw;
      grid-template-rows: 1fr 2fr 1fr;

      h3{
        font-size: 5vw;
      }

      p{
        font-size: 3.8vw;
      }

      h2{
        font-size: 5.8vw;
      }
    }
  }
}
</style>
