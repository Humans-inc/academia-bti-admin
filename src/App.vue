<script setup></script>

<template>
  <div class="main-content">
    <div class="main-content__nav">Navigation</div>
    <div class="main-content__content">
      <div class="custom-form">Form here</div>
      <div class="custom-list">Items List</div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.main-content {
  * {
    font-family: Arial, Helvetica, sans-serif;
    padding: 20px;
    border: 1px solid gray;
  }

  display: grid;
  grid-template-columns: 250px 1fr;
  width: 100%;
  max-width: 1920px;
  min-height: 100vh;

  &__content {
    display: grid;
    grid-template-columns: repeat(2, 50%);
  }
}
</style>
