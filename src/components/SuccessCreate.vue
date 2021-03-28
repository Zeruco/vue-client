<template>
  <div class="backdrop" @click.self="closeModal">
    <div class="success-window">
        <h1>Клиент успешно создан!</h1>
    </div>
  </div>
</template>

<script>
export default {
  name: "SuccessCreate",
  methods : {
    closeModal() {
      this.$emit('close');
    }
  }
}
</script>

<style lang="sass" scoped>


  .backdrop
    top: 0
    position: fixed
    background: rgba(0, 0, 0, 0.5)
    width: 100%
    height: 100%
    left: 0
    z-index: 2

    & .success-window
      @media (max-width: 520px)
        margin: 100px 20px 0 20px

      border-radius: 10px
      background-color: white
      max-width: 500px
      height: 200px
      margin: 100px auto 0 auto
      box-shadow: 0 0 10px black
      & h1
        padding: 70px 10px 0 10px
        font-size: 24px
        letter-spacing: 1px
        font-weight: 900

</style>