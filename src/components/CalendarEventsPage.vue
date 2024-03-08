<template>
  <div class="main-content__content">
    <div class="form">
      <h2 class="form__title">Добавить событие</h2>
      <form class="form__wrapper" @submit.prevent="handleFormSubmitted">
        <div class="form__group">
          <div class="form__group-label">Даты</div>
          <VueDatePicker
            v-model="date"
            range
            :enable-time-picker="false"
            multi-calendars
            auto-apply
            locale="ru"
            :format="format"
          />
        </div>
        <v-text-field
          label="Название события"
          variant="outlined"
          placeholder="Второй шаг в сметное дело"
          v-model="formData.title"
        ></v-text-field>
        <v-textarea
          label="Описание события"
          variant="outlined"
          placeholder="Длинное описание в несколько строк"
          v-model="formData.description"
        ></v-textarea>
        <v-text-field
          label="Ссылка на событие"
          variant="outlined"
          placeholder="https://..."
          v-model="formData.buttonLink"
        ></v-text-field>
        <v-text-field
          label="Ссылка на картинку"
          variant="outlined"
          placeholder="https://..."
          v-model="formData.imgLink"
        ></v-text-field>
        <div class="form__group">
          <div class="form__group-label">Тип</div>
          <v-text-field
            label="Тип события"
            variant="outlined"
            placeholder="Книга"
            v-model="formData.type.text"
          ></v-text-field>
          <v-text-field
            label="Ссылка на иконку"
            variant="outlined"
            placeholder="https://..."
            v-model="formData.type.icon"
          ></v-text-field>
          <div class="form__group-col-2">
            <div class="color-wrapper">
              Цвет текста
              <v-color-picker :modes="['hex']" v-model="formData.type.textColor"></v-color-picker>
            </div>
            <div class="color-wrapper">
              Цвет фона
              <v-color-picker
                :modes="['hex']"
                v-model="formData.type.backgroundColor"
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
      <h2 class="list__title">Курсы абонемента</h2>
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
import { events } from '../../public/data/calendarEvents';
import { ref } from 'vue';
import draggable from 'vuedraggable';
import VueDatePicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css';

const listData = ref([...events]);
const formData = ref({
  title: '',
  description: '',
  buttonLink: '',
  startDate: '',
  endDate: '',
  imgLink: '',
  type: {
    text: '',
    icon: '',
    textColor: '',
    backgroundColor: ''
  }
});
const date = ref([new Date(), new Date()]);

const format = ([dateStart, dateEnd]) => {
  const day1 = dateStart.getDate();
  const month1 = dateStart.getMonth() + 1;
  const year1 = dateStart.getFullYear();

  const day2 = dateEnd.getDate();
  const month2 = dateEnd.getMonth() + 1;
  const year2 = dateEnd.getFullYear();

  const addZero = (num) => {
    return +num < 10 ? `0${num}` : `${num}`;
  };

  return `${addZero(day1)}.${addZero(month1)}.${year1} - ${addZero(day2)}.${addZero(month2)}.${year2}`;
};

const handleFormSubmitted = () => {
  const [startDate, endDate] = date.value;
  if (formData.value.title.length) {
    const newData = {
      id: new Date().getTime(),
      title: formData.value.title,
      description: formData.value.description,
      buttonLink: formData.value.buttonLink,
      startDate: startDate.toISOString(),
      endDate: endDate.toISOString(),
      imgLink: formData.value.imgLink,
      type: {
        text: formData.value.type.text,
        icon: formData.value.type.icon,
        textColor: formData.value.type.textColor,
        backgroundColor: formData.value.type.backgroundColor
      }
    };
    listData.value.unshift(newData);
    date.value = [new Date(), new Date()];
    formData.value = {
      title: '',
      description: '',
      buttonLink: '',
      startDate: '',
      endDate: '',
      imgLink: '',
      type: {
        text: '',
        icon: '',
        textColor: '',
        backgroundColor: ''
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
</style>
