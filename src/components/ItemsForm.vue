<template>
  <div class="form">
    <h2 class="form__title">Form</h2>
    <div class="form__wrapper">
      <v-text-field
        label="Title"
        variant="outlined"
        placeholder="johndoe@gmail.com"
        hint="Enter your password to access this website"
      ></v-text-field>
      <v-switch label="Is hot" inset></v-switch>
      <v-textarea
        label="Label"
        variant="outlined"
        placeholder="johndoe@gmail.com"
        hint="Enter your password to access this website"
      ></v-textarea>
      <v-color-picker></v-color-picker>
      <!-- <v-color-picker :modes="['hex']"></v-color-picker> -->
      <v-date-picker></v-date-picker>
      <v-btn size="x-large" variant="tonal">Save</v-btn>
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
    gap: 15px;
  }
}
</style>
