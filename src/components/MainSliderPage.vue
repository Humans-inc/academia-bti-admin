<template>
  <div class="main-content__content">
    <div class="form">
      <h2 class="form__title">Добавить</h2>
      <form class="form__wrapper" @submit.prevent="handleFormSubmitted">
        <v-text-field
          label="Название"
          variant="outlined"
          placeholder="Второй шаг в сметное дело"
          v-model="formData.title"
          required
        ></v-text-field>
        <v-textarea
          label="Описание"
          variant="outlined"
          placeholder="Длинное описание в несколько строк"
          v-model="formData.description"
          required
        ></v-textarea>
        <div class="form__group">
          <div class="form__group-label">Главная кнопка</div>
          <v-text-field
            label="Текст для кнопки"
            variant="outlined"
            placeholder="Перейти..."
            v-model="formData.mainButton.text"
            required
          ></v-text-field>
          <v-text-field
            label="Ссылка для кнопки"
            variant="outlined"
            placeholder="https://..."
            v-model="formData.mainButton.link"
            required
          ></v-text-field>
          <div class="form__group-col-2">
            <div class="color-wrapper">
              Цвет текста
              <v-color-picker
                :modes="['hex']"
                v-model="formData.mainButton.textColor"
              ></v-color-picker>
            </div>
            <div class="color-wrapper">
              Цвет фона
              <v-color-picker
                :modes="['hex']"
                v-model="formData.mainButton.bgColor"
              ></v-color-picker>
            </div>
          </div>
        </div>
        <v-text-field
          label="Ссылка на фоновую картинку"
          variant="outlined"
          placeholder="https://..."
          v-model="formData.bgImage"
          required
        ></v-text-field>
        <div class="form__group">
          <div class="form__group-label">Бонус</div>
          <v-text-field
            label="Текст"
            variant="outlined"
            placeholder="После регистрации Вы получите..."
            v-model="formData.bonus.bonusText"
            required
          ></v-text-field>
          <v-text-field
            label="Ссылка для картинки"
            variant="outlined"
            placeholder="https://..."
            v-model="formData.bonus.bonusImg"
            required
          ></v-text-field>
        </div>
        <div class="form__group">
          <div class="form__group-label">Ярлыки</div>
          <v-text-field
            label="Ярлык 1"
            variant="outlined"
            placeholder="Книга"
            v-model="formData.labels[0].text"
            required
          ></v-text-field>
          <v-text-field
            label="Ссылка на иконку"
            variant="outlined"
            placeholder="https://..."
            v-model="formData.labels[0].icon"
            required
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
        <v-btn size="x-large" variant="tonal" class="v-btn text-primary" type="submit"
          >Добавить</v-btn
        >
      </form>
    </div>
    <div class="list">
      <h2 class="list__title">Слайдер</h2>
      <draggable v-model="listData" class="list__wrapper" item-key="main">
        <template #item="{ element }">
          <div class="list-item" :itemKey="element.id">
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
import { mainSliderData } from '../../public/data/mainSlider';
import { ref } from 'vue';
import draggable from 'vuedraggable';

const listData = ref([...mainSliderData]);
const formData = ref({
  title: '',
  description: '',
  mainButton: {
    link: '',
    text: '',
    textColor: '',
    bgColor: ''
  },
  bgImage: '',
  bonus: {
    bonusImg: '',
    bonusText: ''
  },
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
  ]
});

const handleFormSubmitted = () => {
  if (formData.value.title.length) {
    const newData = {
      title: formData.value.title,
      description: formData.value.description,
      mainButton: {
        link: formData.value.mainButton.link,
        text: formData.value.mainButton.text,
        textColor: formData.value.mainButton.textColor,
        bgColor: formData.value.mainButton.bgColor
      },
      bgImage: formData.value.bgImage,
      bonus: {
        bonusImg: formData.value.bonus.bonusImg,
        bonusText: formData.value.bonus.bonusText
      },
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
        }
      ]
    };
    listData.value.unshift(newData);
    formData.value = {
      title: '',
      description: '',
      mainButton: {
        link: '',
        text: '',
        textColor: '',
        bgColor: ''
      },
      bgImage: '',
      bonus: {
        bonusImg: '',
        bonusText: ''
      },
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
      ]
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

  img {
    width: 100%;
    height: auto;
    display: block;
    margin-bottom: 16px;
  }
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
