<template>
  <div class="main-content__content">
    <div class="form">
      <h2 class="form__title">Добавить бесплатный материал</h2>
      <form class="form__wrapper" @submit.prevent="handleFormSubmitted">
        <v-text-field
          label="Название курса"
          variant="outlined"
          placeholder="Первый шаг в сметное дело"
          v-model="formData.title"
        ></v-text-field>
        <v-text-field
          label="Ссылка на картинку"
          variant="outlined"
          placeholder="https://..."
          v-model="formData.img"
        ></v-text-field>
        <div class="form__group">
          <div class="form__group-label">Тип</div>
          <v-text-field
            label="Тип материала"
            variant="outlined"
            placeholder="Книга"
            v-model="formData.label.text"
          ></v-text-field>
          <v-text-field
            label="Ссылка на иконку"
            variant="outlined"
            placeholder="https://..."
            v-model="formData.label.iconLink"
          ></v-text-field>
          <div class="form__group-col-2">
            <div class="color-wrapper">
              Цвет текста
              <v-color-picker :modes="['hex']" v-model="formData.label.textColor"></v-color-picker>
            </div>
            <div class="color-wrapper">
              Цвет фона
              <v-color-picker :modes="['hex']" v-model="formData.label.bgColor"></v-color-picker>
            </div>
          </div>
        </div>
        <v-textarea
          label="Описание"
          variant="outlined"
          placeholder="Описание курса"
          v-model="formData.description"
        ></v-textarea>
        <v-text-field
          label="Ссылка для кнопки"
          variant="outlined"
          placeholder="https://..."
          v-model="formData.buttonLink"
        ></v-text-field>
        <v-switch
          label="Для опытных"
          inset
          v-model="formData.isForSkilled"
          color="primary"
        ></v-switch>
        <v-btn size="x-large" variant="tonal" class="v-btn text-primary" type="submit"
          >Добавить</v-btn
        >
      </form>
    </div>
    <div class="list">
      <h2 class="list__title">Бесплатные материалы</h2>
      <draggable v-model="listData" class="list__wrapper" item-key="free-courses">
        <template #item="{ element }">
          <div class="list-item" itemKey="element.id">
            <div class="list-item__name">{{ element.title }}</div>
            <div class="list-item__label">
              {{ element.label.text ? `${element.label.text},` : '' }}
              {{ element.isForSkilled ? 'Для опытных' : 'Для новичков' }}
            </div>
            <div class="list-item__link">{{ element.description }}</div>
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
import { freeCourses } from '../../public/data/freeCourses';
import { ref } from 'vue';
import draggable from 'vuedraggable';
const listData = ref([...freeCourses]);
const formData = ref({
  title: '',
  img: '',
  description: '',
  buttonLink: '',
  isForSkilled: false,
  label: {
    text: '',
    textColor: '',
    bgColor: '',
    iconLink: ''
  }
});

const handleFormSubmitted = () => {
  if (formData.value.title.length && formData.value.buttonLink.length) {
    const newData = {
      id: new Date().getTime(),
      title: formData.value.title,
      img: formData.value.img,
      description: formData.value.description,
      buttonLink: formData.value.buttonLink,
      isForSkilled: formData.value.isForSkilled,
      label: {
        text: formData.value.text,
        textColor: formData.value.textColor,
        bgColor: formData.value.bgColor,
        iconLink: formData.value.iconLink
      }
    };
    listData.value.unshift(newData);
    formData.value = {
      title: '',
      img: '',
      description: '',
      buttonLink: '',
      isForSkilled: false,
      label: {
        text: '',
        textColor: '',
        bgColor: '',
        iconLink: ''
      }
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

<style scoped lang="scss">
.form {
  padding: 40px;

  &__title {
    margin-bottom: 20px;
  }

  &__wrapper {
    display: flex;
    flex-direction: column;
  }

  &__group {
    position: relative;
    margin-bottom: 22px;
    padding: 20px;
    border: 1px solid #ababab;
    border-radius: 4px;
  }
  &__group-label {
    position: absolute;
    bottom: 100%;
    transform: translate(0, 50%);
    font-size: 14px;
    font-weight: 600;
    padding: 0 4px;
    background: #fff;
  }
  &__group-col-2 {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 10px;
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
  &__label {
    font-style: italic;
    margin-bottom: 10px;
  }
}
</style>
