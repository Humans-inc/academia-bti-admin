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
          required
        ></v-text-field>
        <v-text-field
          label="Ссылка на картинку"
          variant="outlined"
          placeholder="https://..."
          v-model="formData.img"
          required
        ></v-text-field>
        <div class="form__group">
          <div class="form__group-label">Тип</div>
          <v-text-field
            label="Тип материала"
            variant="outlined"
            placeholder="Книга"
            v-model="formData.label.text"
            required
          ></v-text-field>
          <v-text-field
            label="Ссылка на иконку"
            variant="outlined"
            placeholder="https://..."
            v-model="formData.label.iconLink"
            required
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
          required
        ></v-textarea>
        <v-text-field
          label="Ссылка для кнопки"
          variant="outlined"
          placeholder="https://..."
          v-model="formData.buttonLink"
          required
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
            <div class="course">
              <div class="course__title-wrap">
                <div class="course__title">
                  {{ element.title }}
                </div>
                <div
                  class="course__type"
                  :style="{
                    color: element.label.textColor,
                    backgroundColor: element.label.bgColor
                  }"
                >
                  {{ element.label.text ? `${element.label.text},` : '' }}
                  {{ element.isForSkilled ? 'Для опытных' : 'Для новичков' }}
                </div>
              </div>
              <div class="course__description">
                {{ element.description }}
              </div>
              <div class="course__img">
                <img :src="element.image" alt="" />
              </div>
            </div>
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
    @media (max-width: 600px) {
      grid-template-columns: 1fr;
    }
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

.course {
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: flex-start;
  padding: 42px;
  border-radius: 12px;
  background: #f6f9fc;
  height: 100%;
  margin-bottom: 16px;
}
@media (max-width: 800px) {
  .course {
    padding: 23px 18px;
  }
}
.course__title-wrap {
  position: relative;
  z-index: 1;
  min-height: 154px;
}
@media (max-width: 800px) {
  .course__title-wrap {
    min-height: 117px;
  }
}
.course__title {
  max-width: 70%;
  font-family: 'Inter', sans-serif;
  font-size: 22px;
  font-weight: 700;
  line-height: 22px;
  margin-bottom: 24px;
  color: #182649;
}
@media (max-width: 800px) {
  .course__title {
    font-size: 18px;
    line-height: 18px;
  }
}
.course__type {
  display: inline-block;
  padding: 8px 12px;
  border-radius: 50px;
  font-family: 'Gilroy', sans-serif;
  font-size: 16px;
  font-weight: 700;
  line-height: 16px;
  color: #182649;
}
.course__type_book {
  background: #c9e5ff;
}
.course__type_web {
  background: #b1fea4;
}
.course__type_access {
  background: #ffd8e6;
}
@media (max-width: 800px) {
  .course__type {
    font-size: 12px;
    line-height: 12px;
    padding: 6px 9px;
  }
}
.course__description {
  margin-bottom: 32px;
  font-family: 'Gilroy', sans-serif;
  font-size: 16px;
  font-weight: 500;
  line-height: 21px;
  color: #182649;
}
@media (max-width: 800px) {
  .course__description {
    font-size: 14px;
    line-height: 14px;
  }
}
.course__button {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: auto;
  width: 100%;
  padding: 20px;
  border-radius: 70px;
  font-family: 'Gilroy', sans-serif;
  font-size: 18px;
  font-weight: 700;
  line-height: 22px;
  background: #4e58f7;
  color: #ffffff;
  text-decoration: none;
}
@media (max-width: 800px) {
  .course__button {
    font-size: 18px;
    line-height: 18px;
    padding: 16px;
  }
}
.course__button::before {
  content: '';
  position: absolute;
  top: 6px;
  bottom: 6px;
  left: 6px;
  aspect-ratio: 1;
  border-radius: 500px;
  background: #3f47c9
    url(https://fs.getcourse.ru/fileservice/file/download/a/600892/sc/183/h/4bcd5750eae44ed117b1251971d715eb.svg)
    no-repeat center;
}
.course__img {
  position: absolute;
  top: 16px;
  right: 16px;
  width: 180px;
  aspect-ratio: 1;
}
@media (max-width: 800px) {
  .course__img {
    width: 124px;
  }
}
.course__img img {
  display: block;
  width: 100%;
  height: 100%;
  -o-object-fit: contain;
  object-fit: contain;
}
</style>
