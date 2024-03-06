<template>
  <div class="form">
    <h2 class="form__title">Добавить отзыв</h2>
    <form class="form__wrapper" @submit.prevent="submitForm">
      <v-text-field
        label="Имя ученика"
        variant="outlined"
        placeholder="Сергей Петрович"
        v-model="formData.name"
      ></v-text-field>
      <v-text-field
        label="Ссылка на отзыв"
        variant="outlined"
        placeholder="https://www.youtube.com/embed/"
        hint="Ссылка на Youtube или Vimeo"
        v-model="formData.link"
      ></v-text-field>
      <v-btn size="x-large" variant="tonal" class="v-btn text-primary" type="submit"
        >Добавить</v-btn
      >
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        name: '',
        link: ''
      }
    }
  },
  methods: {
    submitForm() {
      if (this.formData.name.length && this.formData.link.length) {
        this.formData.id = new Date().getTime()
        this.$emit('form-submitted', this.formData)
        // send to server

      } else {
        alert('Заполните все поля')
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.form {
  padding: 40px;

  &__title {
    margin-bottom: 20px;
  }

  &__wrapper {
    display: flex;
    flex-direction: column;
  }

  .v-btn {
    margin-top: 20px;
  }
}
</style>
