<template>
  <div class="list">
    <h2 class="list__title">{{ listTitle }}</h2>
    <div class="list__wrapper">
      <div class="list-item">
        <div class="list-item__name">Item Name</div>
        <v-btn class="list-item__btn" variant="tonal">Удалить</v-btn>
      </div>
      <div class="list-item">
        <div class="list-item__name">Item Name</div>
        <v-btn class="list-item__btn" variant="tonal">Удалить</v-btn>
      </div>
      <div class="list-item">
        <div class="list-item__name">Item Name</div>
        <v-btn class="list-item__btn" variant="tonal">Удалить</v-btn>
      </div>
      <div class="list-item">
        <div class="list-item__name">Item Name</div>
        <v-btn class="list-item__btn" variant="tonal">Удалить</v-btn>
      </div>
      <div class="list-item">
        <div class="list-item__name">Item Name</div>
        <v-btn class="list-item__btn" variant="tonal">Удалить</v-btn>
      </div>
      <div class="list-item">
        <div class="list-item__name">Item Name</div>
        <v-btn class="list-item__btn" variant="tonal">Удалить</v-btn>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    listTitle: String
  }
}
</script>

<style lang="scss" scoped>
.list {
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

.list-item {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  padding: 20px;
  border: 1px solid rgb(var(--v-theme-primary));
  &__btn {
    align-self: flex-end;
  }
}
</style>
