<template>
  <div class="form">
    <h2 class="form__title">Добавить отзыв</h2>
    <div class="form__wrapper">
      <v-text-field
        label="Имя ученика"
        variant="outlined"
        placeholder="Сергей Петрович"
      ></v-text-field>
      <v-text-field
        label="Ссылка на отзыв"
        variant="outlined"
        placeholder="https://www.youtube.com/embed/"
        hint="Ссылка на Youtube или Vimeo"
      ></v-text-field>
      <v-btn size="x-large" variant="tonal" class="v-btn text-primary">Добавить</v-btn>
    </div>
  </div>
</template>

<script></script>

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
}
</style>
