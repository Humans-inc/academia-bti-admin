<template>
  <div class="main-content__content">
    <div class="form">
      <h2 class="form__title">Добавить эксперта</h2>
      <form class="form__wrapper" @submit.prevent="handleFormSubmitted">
        <v-text-field
          label="Имя эксперта"
          variant="outlined"
          placeholder="Сергей Петрович"
          v-model="formData.name"
        ></v-text-field>
        <v-text-field
          label="Ссылка на фото"
          variant="outlined"
          placeholder="https://..."
          hint="Ссылка на фото, загруженное на GetCourse"
          v-model="formData.link"
        ></v-text-field>
        <v-btn size="x-large" variant="tonal" class="v-btn text-primary" type="submit"
          >Добавить</v-btn
        >
      </form>
    </div>
    <div class="list">
      <h2 class="list__title">Эксперты</h2>
      <draggable v-model="listData" class="list__wrapper" item-key="experts">
        <template #item="{ element }">
          <div class="list-item" itemKey="element.id">
            <div class="list-item__name">{{ element.name }}</div>
            <div class="list-item__link">{{ element.link }}</div>
            <v-btn class="list-item__btn" variant="tonal" @click="handleRemoveItem(index)"
              >Удалить</v-btn
            >
          </div>
        </template>
      </draggable>
    </div>
  </div>
</template>

<script setup>
import { videoReviews } from '../../public/data/video-reviews';
import { ref } from 'vue';
import draggable from 'vuedraggable';

const listData = ref([...videoReviews]);
const formData = ref({
  name: '',
  link: ''
});

const handleFormSubmitted = () => {
  if (formData.value.name.length && formData.value.link.length) {
    const newData = {
      id: new Date().getTime(),
      name: formData.value.name,
      link: formData.value.link
    };
    listData.value.unshift(newData);
    formData.value.name = '';
    formData.value.link = '';
    // send to server
  } else {
    alert('Заполните все поля');
  }
};

const handleRemoveItem = (index) => {
  listData.value.splice(index, 1);
};
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
  border-radius: 4px;
  &__name {
    font-size: 18px;
    font-weight: 600;
  }
  &__link {
    margin-bottom: 15px;
    max-width: 90%;
  }
  &__btn {
    align-self: flex-end;
  }
}
</style>
