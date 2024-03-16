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
          required
        ></v-text-field>
        <!-- description -->
        <v-textarea
          label="Описание"
          variant="outlined"
          placeholder="Длинное описание в несколько строк"
          v-model="formData.description"
          required
        ></v-textarea>
        <!-- buttonLink -->
        <v-text-field
          label="Ссылка для кнопки"
          variant="outlined"
          placeholder="https://..."
          v-model="formData.buttonLink"
          required
        ></v-text-field>
        <!-- picture -->
        <v-text-field
          label="Ссылка на картинку"
          variant="outlined"
          placeholder="https://..."
          v-model="formData.image"
          required
        ></v-text-field>
        <!-- price -->
        <div class="form__group">
          <div class="form__group-label">Цена</div>
          <v-text-field
            label="Полная"
            variant="outlined"
            placeholder="10000"
            v-model="formData.price.full"
            @input="handleInput"
            @blur="handleInput"
            required
          ></v-text-field>
          <v-text-field
            label="Зачеркнутая"
            variant="outlined"
            placeholder="10000000"
            v-model="formData.price.del"
            @input="handleInput"
            @blur="handleInput"
            required
          ></v-text-field>
          <v-text-field
            label="В месяц"
            variant="outlined"
            placeholder="1000"
            v-model="formData.price.loan"
            @input="handleInput"
            @blur="handleInput"
            required
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
            required
          ></v-text-field>
          <v-text-field
            label="Ссылка на картинку"
            variant="outlined"
            placeholder="https://..."
            v-model="formData.points[0].img"
            required
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
        <!-- tags -->
        <div class="form__group tags">
          <div class="form__group-label">Теги</div>
          <div class="tags__form">
            <v-text-field
              v-model="tagInput"
              variant="outlined"
              label="Введите тег"
              placeholder="Для новичков"
              required
            ></v-text-field>
            <v-btn
              @click="addTag"
              type="button"
              variant="tonal"
              style="margin-top: 0; height: 56px"
            >
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
      <draggable v-model="listData" class="list__wrapper" item-key="courses">
        <template #item="{ element }">
          <div class="list-item" :itemKey="element.id">
            <div class="course">
              <div class="course__top">
                <div>
                  <div class="course__title">{{ element.title }}</div>
                  <div class="course__bages">
                    <div
                      class="course__bage"
                      :style="{
                        color: element.labels[0]?.textColor,
                        backgroundColor: element.labels[0]?.bgColor,
                        display: element.labels[0] ? 'flex' : 'none'
                      }"
                    >
                      <img
                        :src="element.labels[0]?.img"
                        alt=""
                        :style="{ display: element.labels[0]?.img?.length ? 'block' : 'none' }"
                      />
                      {{ element.labels[0]?.text }}
                    </div>
                    <div
                      class="course__bage"
                      :style="{
                        color: element.labels[1]?.textColor,
                        backgroundColor: element.labels[1]?.bgColor,
                        display: element.labels[1] ? 'flex' : 'none'
                      }"
                    >
                      <img
                        :src="element.labels[1]?.img"
                        alt=""
                        :style="{ display: element.labels[1]?.img?.length ? 'block' : 'none' }"
                      />
                      {{ element.labels[1]?.text }}
                    </div>
                  </div>
                  <div class="course__subtitle">{{ element.description }}</div>
                </div>
                <div class="course__img">
                  <img :src="element.image" alt="" />
                </div>
              </div>
              <div class="course__data">
                <div
                  class="course__data-item"
                  :style="{
                    display: element.points[0] ? 'flex' : 'none'
                  }"
                >
                  <img :src="element.points[0]?.img || ''" alt="" />
                  {{ element.points[0]?.text || '' }}
                </div>
                <div
                  class="course__data-item"
                  :style="{
                    display: element.points[1] ? 'flex' : 'none'
                  }"
                >
                  <img :src="element.points[1]?.img || ''" alt="" />
                  {{ element.points[1]?.text || '' }}
                </div>
                <div
                  class="course__data-item"
                  :style="{
                    display: element.points[2] ? 'flex' : 'none'
                  }"
                >
                  <img :src="element.points[2]?.img || ''" alt="" />
                  {{ element.points[2]?.text || '' }}
                </div>
                <div
                  class="course__data-item"
                  :style="{
                    display: element.points[3] ? 'flex' : 'none'
                  }"
                >
                  <img :src="element.points[3]?.img || ''" alt="" />
                  {{ element.points[3]?.text || '' }}
                </div>
              </div>
              <div class="course__bottom">
                <div class="course__price">
                  {{ addCurrency(element.price.loan) }}/мес.
                  <div class="course__price-total">
                    Всей суммой {{ addCurrency(element.price.full)
                    }}<del>{{ addCurrency(element.price.del) }}</del>
                  </div>
                </div>
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

const handleInput = (e) => {
  e.target.value = e.target.value.replace(/\D/gi, '');
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
        }
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
        }
      ],
      price: {
        del: formData.value.price.del.replace(/\D/gi, ''),
        full: formData.value.price.full.replace(/\D/gi, ''),
        loan: formData.value.price.loan.replace(/\D/gi, '')
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

const addCurrency = (num) => {
  let number = Number(num);
  return number.toLocaleString('ru-RU', {
    style: 'currency',
    currency: 'RUB',
    maximumFractionDigits: 0,
    minimumFractionDigits: 0
  });
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
.course {
  padding: 50px;
  border-radius: 12px;
  background: #f6f9fc;
  display: flex;
  flex-direction: column;
  height: 100%;
  position: relative;
  margin-bottom: 16px;
}
@media (max-width: 600px) {
  .course {
    padding: 20px;
  }
}
.course__top {
  display: flex;
  justify-content: stretch;
  margin-bottom: 25px;
}
.course__top > div:first-child {
  width: 60%;
  flex-shrink: 0;
}
@media (max-width: 600px) {
  .course__top > div:first-child {
    width: 100%;
    flex-shrink: 1;
  }
}
.course__title {
  margin-bottom: 22px;
  font-family: Inter;
  font-size: 32px;
  font-weight: 700;
  line-height: 32px;
  color: #182649;
}
@media (max-width: 600px) {
  .course__title {
    width: 100%;
    max-width: 60%;
    min-height: 106px;
    padding-bottom: 36px;
    font-size: 18px;
    line-height: 18px;
  }
}
.course__bages {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  flex-wrap: wrap;
  gap: 10px;
  margin-bottom: 33px;
}
@media (max-width: 600px) {
  .course__bages {
    margin-bottom: 19px;
  }
}
.course__bage {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 2px;
  padding: 7px 13px;
  border-radius: 50px;
  font-family: Gilroy;
  font-size: 16px;
  font-weight: 700;
  line-height: 16px;
  color: #182649;
  background: #c9e5ff;
}
@media (max-width: 600px) {
  .course__bage {
    font-size: 12px;
    line-height: 12px;
  }
}

.course__bage img {
  display: block;
  width: 18px;
  height: 18px;
  object-fit: contain;
}
@media (max-width: 600px) {
  .course__bage_hot::before {
    width: 12px;
    height: 12px;
  }
}
.course__subtitle {
  font-family: Inter;
  font-size: 18px;
  font-weight: 700;
  line-height: 23px;
  color: #182649;
}
@media (max-width: 600px) {
  .course__subtitle {
    font-size: 14px;
    line-height: 14px;
  }
}
@media (max-width: 1200px) {
  .course__img {
    max-height: 250px;
    flex: 1 1 100%;
  }
}
@media (max-width: 600px) {
  .course__img {
    position: absolute;
    top: 15px;
    right: 6px;
    max-height: 106px;
    width: 132px;
  }
}
.course__img img {
  width: 100%;
  height: 100%;
  -o-object-fit: contain;
  object-fit: contain;
}
.course__data {
  margin-bottom: 40px;
}
@media (max-width: 600px) {
  .course__data {
    margin-bottom: 25px;
  }
}
.course__data-item {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  gap: 16px;
  margin-bottom: 20px;
  font-family: Gilroy;
  font-weight: 500;
  font-size: 16px;
  line-height: 130%;
  color: #182649;
}
@media (max-width: 600px) {
  .course__data-item {
    font-size: 12px;
    align-items: flex-start;
  }
}
.course__data-item img {
  flex-shrink: 0;
  display: block;
  width: 48px;
  height: 48px;
  object-fit: contain;
  filter: drop-shadow(2px 2px 10px rgba(64, 71, 88, 0.15));
}
@media (max-width: 600px) {
  .course__data-item img {
    width: 36px;
    height: 36px;
    position: relative;
    top: -4px;
  }
}

.course__bottom {
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  gap: 20px;
  margin-top: auto;
}
@media (max-width: 600px) {
  .course__bottom {
    flex-direction: column;
    align-items: flex-start;
    justify-content: flex-end;
  }
}
.course__price {
  display: flex;
  flex-direction: column;
  flex-shrink: 0;
  gap: 14px;
  font-family: Inter;
  font-weight: 700;
  font-size: 32px;
  line-height: 100%;
  color: #4e58f7;
}
@media (max-width: 600px) {
  .course__price {
    font-size: 22px;
    gap: 10px;
  }
}
.course__price-total {
  font-weight: 500;
  font-size: 18px;
  line-height: 100%;
  color: #182649;
}
.course__price-total del {
  display: inline-block;
  margin-left: 5px;
  opacity: 0.5;
}
@media (max-width: 600px) {
  .course__price-total {
    font-size: 14px;
  }
}
.course__button {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  max-width: 270px;
  padding: 20px;
  border-radius: 100px;
  font-family: Gilroy;
  font-weight: 700;
  font-size: 18px;
  color: #fff;
  background: #4e58f7;
  text-decoration: none;
}
@media (max-width: 600px) {
  .course__button {
    padding: 13px;
    font-size: 14px;
  }
}
.course__button svg {
  position: absolute;
  top: 50%;
  right: 20px;
  display: block;
  transform: translate(0, -50%);
}
</style>
