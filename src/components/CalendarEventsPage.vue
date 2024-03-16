<template>
  <div class="main-content__content">
    <div class="form">
      <h2 class="form__title">Добавить событие</h2>
      <form class="form__wrapper" @submit.prevent="handleFormSubmitted">
        <v-text-field
          label="Название события"
          variant="outlined"
          placeholder="Второй шаг в сметное дело"
          v-model="formData.title"
          required
        ></v-text-field>
        <v-textarea
          label="Описание события"
          variant="outlined"
          placeholder="Длинное описание в несколько строк"
          v-model="formData.description"
          required
        ></v-textarea>
        <v-text-field
          label="Ссылка на событие"
          variant="outlined"
          placeholder="https://..."
          v-model="formData.buttonLink"
          required
        ></v-text-field>
        <v-text-field
          label="Ссылка на картинку"
          variant="outlined"
          placeholder="https://..."
          v-model="formData.imgLink"
          required
        ></v-text-field>
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
        <div class="form__group">
          <div class="form__group-label">Тип</div>
          <v-text-field
            label="Тип события"
            variant="outlined"
            placeholder="Книга"
            v-model="formData.type.text"
            required
          ></v-text-field>
          <v-text-field
            label="Ссылка на иконку"
            variant="outlined"
            placeholder="https://..."
            v-model="formData.type.icon"
            required
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
            <div class="event">
              <div class="event__date">
                {{
                  `${new Date(element.startDate).getDate()}${new Date(element.startDate).getDate() !== new Date(element.endDate).getDate() ? ` - ${new Date(element.endDate).getDate()}` : ''}`
                }}<small>{{
                  `${monthFormatter.format(new Date(element.startDate))}${new Date(element.startDate).getMonth() === new Date(element.endDate).getMonth() ? '' : ` - ${monthFormatter.format(new Date(element.endDate))}`}`
                }}</small>
              </div>
              <div class="event__icon"></div>
              <div class="event__data">
                <div class="event__title">{{ element.title }}</div>
                <div class="event__bages">
                  <div
                    class="event__bage"
                    :style="{
                      color: element.type.textColor,
                      backgroundColor: element.type.backgroundColor
                    }"
                  >
                    <img
                      :src="element.type.icon"
                      alt=""
                      :style="{ display: element.type.icon.length ? 'block' : 'none' }"
                    />
                    {{ element.type.text }}
                  </div>
                </div>
                <div class="event__descr">{{ element.description }}</div>
                <img
                  :src="element.imgLink"
                  alt=""
                  :style="{ display: element.imgLink.length ? 'block' : 'none' }"
                />
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

const monthFormatter = new Intl.DateTimeFormat('ru-RU', { month: 'long' });
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
.event {
  display: grid;
  grid-template-columns: 80px 22px 1fr;
  width: 100%;
}

.event:not(:last-child) {
  padding-bottom: 89px;
}
@media (max-width: 1000px) {
  .event:not(:last-child) {
    padding-bottom: 60px;
  }
}
.event:nth-child(odd) .event__data > img {
  transform: rotate(-6deg);
}
.event:nth-child(even) .event__data > img {
  transform: rotate(6deg);
}
@media (max-width: 1000px) {
  .event:nth-child(even) .event__data > img {
    transform: rotate(-6deg);
  }
}
.event:last-child .event__icon::before {
  display: none;
}

@media (max-width: 600px) {
  .event_hot .event__icon::after {
    left: calc(50% - 11px);
    width: 22px;
    height: 22px;
  }
}
.event__date {
  display: flex;
  flex-direction: column;
  font-family: 'Inter', sans-serif;
  font-size: 22px;
  font-weight: 700;
  line-height: normal;
  color: #4e58f7;
}

.event__date small {
  font-size: 12px;
}

.event__icon {
  position: relative;
  margin-bottom: -89px;
}
@media (max-width: 1000px) {
  .event__icon {
    margin-bottom: -60px;
  }
}
.event__icon::before {
  content: '';
  position: absolute;
  top: 0px;
  bottom: 0px;
  left: calc(50% - 1px);
  display: block;
  width: 2px;
  background: #4e58f7;
}
.event__icon::after {
  content: '';
  position: absolute;
  top: 0px;
  left: calc(50% - 11px);
  display: block;
  width: 22px;
  height: 22px;
  background: url(https://fs.getcourse.ru/fileservice/file/download/a/1436/sc/322/h/a081fa4c62fa217cc653d7abe6a61cc6.svg)
    no-repeat center/contain;
}
@media (max-width: 600px) {
  .event__icon::after {
    left: calc(50% - 8px);
    display: block;
    width: 16px;
    height: 16px;
  }
}
.event__data {
  position: relative;
  padding: 0 20px;
  display: flex;
  flex-direction: column;
}

@media (max-width: 1000px) {
  .event__data {
    padding: 0 20px;
  }
}
.event__data > img {
  top: -5px;
  right: 0;
  width: 96px;
  aspect-ratio: 1.5;
  height: auto;
  -o-object-fit: cover;
  object-fit: cover;
  border-radius: 6px;
  overflow: hidden;
  position: relative;
  order: 1;
}
@media (max-width: 1300px) {
  .event__data > img {
    width: 160px;
    aspect-ratio: 1.5;
    height: auto;
  }
}
@media (max-width: 600px) {
  .event__data > img {
    width: 96px;
  }
}
.event__title {
  margin: 19px 0;
  font-family: 'Inter', sans-serif;
  font-size: 26px;
  font-weight: 700;
  line-height: 26px;
  color: #182649;
  order: 2;
}
@media (max-width: 600px) {
  .event__title {
    margin-top: 20px;
    margin-bottom: 17px;
    font-size: 18px;
    line-height: 18px;
  }
}
.event__bages {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  flex-wrap: wrap;
  gap: 10px;
  margin-bottom: 19px;
  order: 3;
}
@media (max-width: 1000px) {
  .event__bages {
    display: none;
  }
}
.event__bage {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  padding: 8px 12px;
  border-radius: 32px;
  font-family: 'Gilroy', sans-serif;
  font-size: 16px;
  font-weight: 700;
  line-height: 16px;
  letter-spacing: 0em;
  text-align: left;
  color: #182649;
}
.event__bage_hot {
  color: #fff;
  background: #f43d3d;
}
.event__bage_hot::before {
  content: '';
  display: block;
  width: 18px;
  height: 18px;
  background: url(https://fs.getcourse.ru/fileservice/file/download/a/1436/sc/369/h/247a26cb34dd607a5a51d9ebd3fccbd0.svg)
    no-repeat center/contain;
}

.event__descr {
  margin-bottom: 16px;
  font-family: Gilroy;
  font-size: 18px;
  font-weight: 500;
  line-height: 23px;
  color: #182649;
  order: 4;
}
@media (max-width: 1000px) {
  .event__descr {
    position: relative;
  }
}
@media (max-width: 600px) {
  .event__descr {
    margin-bottom: 10px;
    font-size: 12px;
    line-height: 12px;
  }
}
.event__link {
  position: relative;
  width: 100%;
  max-width: 270px;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 11px;
  border-radius: 50px;
  font-family: Gilroy;
  font-size: 18px;
  font-weight: 700;
  line-height: 22px;
  color: #fff;
  background: #4e58f7;
  text-decoration: none;
  order: 5;
}
@media (max-width: 1000px) {
  .event__link {
    position: relative;
  }
}
@media (max-width: 600px) {
  .event__link {
    max-width: 230px;
    padding: 10px;
    font-size: 14px;
    line-height: 14px;
  }
}
.event__link svg {
  position: absolute;
  top: 50%;
  right: 20px;
  transform: translate(0, -50%);
}
@media (max-width: 600px) {
  .event__link svg {
    right: 14px;
    width: 11px;
    height: auto;
  }
}
</style>
