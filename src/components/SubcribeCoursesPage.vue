<template>
  <div class="main-content__content">
    <div class="form">
      <h2 class="form__title">Добавить курс</h2>
      <form class="form__wrapper" @submit.prevent="handleFormSubmitted">
        <v-text-field
          label="Название курса"
          variant="outlined"
          placeholder="Второй шаг в сметное дело"
          v-model="formData.title"
        ></v-text-field>
        <v-text-field
          label="Цена курса"
          variant="outlined"
          placeholder="100 000"
          v-model="formData.price"
        ></v-text-field>
        <v-text-field
          label="Ссылка на курс"
          variant="outlined"
          placeholder="https://..."
          v-model="formData.link"
        ></v-text-field>
        <v-text-field
          label="Ссылка на картинку курса"
          variant="outlined"
          placeholder="https://..."
          v-model="formData.imgLink"
        ></v-text-field>
        <v-btn size="x-large" variant="tonal" class="v-btn text-primary" type="submit"
          >Добавить</v-btn
        >
      </form>
    </div>
    <div class="list">
      <h2 class="list__title">Курсы абонемента</h2>
      <draggable v-model="listData" class="list__wrapper" item-key="video">
        <template #item="{ element }">
          <div class="list-item" itemKey="element.id">
            <div class="list-item__name">{{ element.title }}</div>
            <div class="list-item__link">{{ element.price }}</div>
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
import { subCourses } from '../../public/data/subCourses';
import { ref } from 'vue';
import draggable from 'vuedraggable';

const listData = ref([...subCourses]);
const formData = ref({
  title: '',
  price: '',
  link: '',
  imgLink: ''
});

const handleFormSubmitted = () => {
  if (formData.value.title.length && formData.value.link.length) {
    const newData = {
      id: new Date().getTime(),
      title: formData.value.title,
      price: formData.value.price,
      link: formData.value.link,
      imgLink: formData.value.imgLink
    };
    listData.value.unshift(newData);
    formData.value = {
      title: '',
      price: '',
      link: '',
      imgLink: ''
    };
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
  @media (max-width: 768px) {
    padding: 20px;
  }

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
