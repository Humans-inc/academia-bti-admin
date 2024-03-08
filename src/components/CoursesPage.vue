<template>
  <div class="main-content__content">
    <div class="form">
      <h2 class="form__title">Добавить курс</h2>
      <form class="form__wrapper" @submit.prevent="handleFormSubmitted">
        <!-- title -->
        <v-text-field
          label="Название"
          variant="outlined"
          placeholder="Второй шаг в сметное дело"
          v-model="formData.title"
        ></v-text-field>
        <!-- description -->
        <v-textarea
          label="Описание"
          variant="outlined"
          placeholder="Длинное описание в несколько строк"
          v-model="formData.description"
        ></v-textarea>
        <!-- buttonLink -->
        <v-text-field
          label="Ссылка для кнопки"
          variant="outlined"
          placeholder="https://..."
          v-model="formData.buttonLink"
        ></v-text-field>
        <!-- picture -->
        <v-text-field
          label="Ссылка на картинку"
          variant="outlined"
          placeholder="https://..."
          v-model="formData.image"
        ></v-text-field>
        <!-- price -->
        <div class="form__group">
          <div class="form__group-label">Цена</div>
          <v-text-field
            label="Полная"
            variant="outlined"
            placeholder="10000"
            v-model="formData.price.full"
          ></v-text-field>
          <v-text-field
            label="Зачеркнутая"
            variant="outlined"
            placeholder="10000000"
            v-model="formData.price.del"
          ></v-text-field>
          <v-text-field
            label="В месяц"
            variant="outlined"
            placeholder="1000"
            v-model="formData.price.loan"
          ></v-text-field>
        </div>
        <!-- points -->
        <div class="form__group">
          <div class="form__group-label">Поинты</div>
          <v-text-field
            label="Поинт 1"
            variant="outlined"
            placeholder="Книга"
            v-model="formData.points[0].text"
          ></v-text-field>
          <v-text-field
            label="Ссылка на картинку"
            variant="outlined"
            placeholder="https://..."
            v-model="formData.points[0].img"
            style="margin-bottom: 20px"
          ></v-text-field>
          <v-text-field
            label="Поинт 2"
            variant="outlined"
            placeholder="Книга"
            v-model="formData.points[1].text"
          ></v-text-field>
          <v-text-field
            label="Ссылка на картинку"
            variant="outlined"
            placeholder="https://..."
            v-model="formData.points[1].img"
            style="margin-bottom: 20px"
          ></v-text-field>
          <v-text-field
            label="Поинт 3"
            variant="outlined"
            placeholder="Книга"
            v-model="formData.points[2].text"
          ></v-text-field>
          <v-text-field
            label="Ссылка на картинку"
            variant="outlined"
            placeholder="https://..."
            v-model="formData.points[2].img"
            style="margin-bottom: 20px"
          ></v-text-field>
          <v-text-field
            label="Поинт 4"
            variant="outlined"
            placeholder="Книга"
            v-model="formData.points[3].text"
          ></v-text-field>
          <v-text-field
            label="Ссылка на картинку"
            variant="outlined"
            placeholder="https://..."
            v-model="formData.points[3].img"
          ></v-text-field>
        </div>
        <!-- labels -->
        <div class="form__group">
          <div class="form__group-label">Ярлыки</div>
          <v-text-field
            label="Ярлык 1"
            variant="outlined"
            placeholder="Книга"
            v-model="formData.labels[0].text"
          ></v-text-field>
          <v-text-field
            label="Ссылка на иконку"
            variant="outlined"
            placeholder="https://..."
            v-model="formData.labels[0].icon"
          ></v-text-field>
          <div class="form__group-col-2 form__group-col-2_border">
            <div class="color-wrapper">
              Цвет текста
              <v-color-picker
                :modes="['hex']"
                v-model="formData.labels[0].textColor"
              ></v-color-picker>
            </div>
            <div class="color-wrapper">
              Цвет фона
              <v-color-picker
                :modes="['hex']"
                v-model="formData.labels[0].bgColor"
              ></v-color-picker>
            </div>
          </div>
          <v-text-field
            label="Ярлык 2"
            variant="outlined"
            placeholder="Книга"
            v-model="formData.labels[1].text"
          ></v-text-field>
          <v-text-field
            label="Ссылка на иконку"
            variant="outlined"
            placeholder="https://..."
            v-model="formData.labels[1].icon"
          ></v-text-field>
          <div class="form__group-col-2">
            <div class="color-wrapper">
              Цвет текста
              <v-color-picker
                :modes="['hex']"
                v-model="formData.labels[1].textColor"
              ></v-color-picker>
            </div>
            <div class="color-wrapper">
              Цвет фона
              <v-color-picker
                :modes="['hex']"
                v-model="formData.labels[1].bgColor"
              ></v-color-picker>
            </div>
          </div>
        </div>
        <!-- tags -->
        <div class="form__group tags">
          <div class="form__group-label">Теги</div>
          <div class="tags__form">
            <v-text-field
              v-model="tagInput"
              variant="outlined"
              label="Введите тег"
              placeholder="Для новичков"
            ></v-text-field>
            <v-btn @click="addTag" type="button" variant="tonal" style="margin-top: 0; height: 56px;">
              <v-icon icon="mdi-plus"></v-icon>
            </v-btn>
          </div>
          <div class="tag-list">
            <div v-for="(tag, index) in formData.tags" :key="index" class="tag">
              {{ tag }}
              <span @click="removeTag(index)">
                <v-icon icon="mdi-close" size="small"></v-icon>
              </span>
            </div>
          </div>
        </div>
        <v-btn size="x-large" variant="tonal" class="v-btn text-primary" type="submit"
          >Добавить</v-btn
        >
      </form>
    </div>
    <div class="list">
      <h2 class="list__title">Курсы</h2>
      <draggable v-model="listData" class="list__wrapper" item-key="video">
        <template #item="{ element }">
          <div class="list-item" itemKey="element.id">
            <div class="list-item__name">{{ element.title }}</div>
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
import { courses } from '../../public/data/courses';
import { ref } from 'vue';
import draggable from 'vuedraggable';

const listData = ref([...courses]);
const formData = ref({
  title: '',
  description: '',
  buttonLink: '',
  image: '',
  tags: [],
  labels: [
    {
      text: '',
      icon: '',
      bgColor: '',
      textColor: ''
    },
    {
      text: '',
      icon: '',
      bgColor: '',
      textColor: ''
    }
  ],
  points: [
    {
      img: '',
      text: ''
    },
    {
      img: '',
      text: ''
    },
    {
      img: '',
      text: ''
    },
    {
      img: '',
      text: ''
    }
  ],
  price: {
    del: null,
    full: null,
    loan: null
  }
});

const tagInput = ref('');
const addTag = () => {
  if (tagInput.value.length && !formData.value.tags.includes(tagInput.value)) {
    formData.value.tags.push(tagInput.value);
    tagInput.value = '';
  }
};
const removeTag = (index) => {
  formData.value.tags.splice(index, 1);
};

const handleFormSubmitted = () => {
  if (formData.value.title.length) {
    const newData = {
      title: formData.value.title,
      description: formData.value.description,
      buttonLink: formData.value.buttonLink,
      image: formData.value.image,
      tags: [...formData.value.tags],
      labels: [
        {
          text: formData.value.labels[0].text,
          icon: formData.value.labels[0].icon,
          bgColor: formData.value.labels[0].bgColor,
          textColor: formData.value.labels[0].textColor
        },
        {
          text: formData.value.labels[1].text,
          icon: formData.value.labels[1].icon,
          bgColor: formData.value.labels[1].bgColor,
          textColor: formData.value.labels[1].textColor
        },
      ],
      points: [
        {
          img: formData.value.points[0].img,
          text: formData.value.points[0].text
        },
        {
          img: formData.value.points[1].img,
          text: formData.value.points[1].text
        },
        {
          img: formData.value.points[2].img,
          text: formData.value.points[2].text
        },
        {
          img: formData.value.points[3].img,
          text: formData.value.points[3].text
        },
      ],
      price: {
        del: formData.value.price.del,
        full: formData.value.price.full,
        loan: formData.value.price.loan
      }
    };
    listData.value.unshift(newData);
    console.log(newData);
    formData.value = {
      title: '',
      description: '',
      buttonLink: '',
      image: '',
      tags: [],
      labels: [
        {
          text: '',
          icon: '',
          bgColor: '',
          textColor: ''
        },
        {
          text: '',
          icon: '',
          bgColor: '',
          textColor: ''
        }
      ],
      points: [
        {
          img: '',
          text: ''
        },
        {
          img: '',
          text: ''
        },
        {
          img: '',
          text: ''
        },
        {
          img: '',
          text: ''
        }
      ],
      price: {
        del: null,
        full: null,
        loan: null
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
    &_border {
      padding-bottom: 20px;
      margin-bottom: 20px;
    }
    @media (max-width: 600px) {
      grid-template-columns: 1fr;
    }
  }

  .color-wrapper {
    display: flex;
    flex-direction: column;
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
.tags {
  &__form {
    display: flex;
    gap: 10px;
  }
  .tag-list {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
  }
  .tag {
    display: flex;
    gap: 8px;
    padding: 4px 8px;
    border-radius: 4px;
    background: #e4e4e4;
    font-size: 18px;
    align-items: center;
    line-height: 18px;
    cursor: pointer;
  }
}
</style>
