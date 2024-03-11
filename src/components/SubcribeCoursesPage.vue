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
          @input="handleInput"
          @blur="handleInput"
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
            <div class="pay-course">
              <div class="pay-course__img">
                <img :src="element.imgLink" alt="" />
              </div>
              <div class="pay-course__data">
                <div class="pay-course__title">{{ element.title }}</div>
                <div class="pay-course__price">
                  <del>{{ addCurrency(element.price.replace(/\D/gi, '')) }}</del
                  ><span class="class">Бесплатно</span>
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
      price: formData.value.price.replace(/\D/gi, ''),
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

const handleInput = (e) => {
  e.target.value = e.target.value.replace(/\D/gi, '');
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

.pay-course {
  display: flex;
  justify-content: flex-start;
  gap: 20px;
}

.pay-course__img {
  width: 96px;
  flex-shrink: 0;
}

.pay-course__img img {
  display: block;
  width: 100%;
  height: 100%;
  object-fit: contain;
}

.pay-course__data {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.pay-course__title {
  font-family: Inter;
  font-size: 16px;
  font-weight: 500;
  line-height: 21px;
  text-align: left;
  color: #182649;
  margin-bottom: 8px;
}

.pay-course__price del {
  display: inline-block;
  margin-right: 12px;
  font-family: Inter;
  font-size: 16px;
  font-weight: 500;
  line-height: 16px;
  letter-spacing: 0em;
  text-align: left;
  color: #182649;
  opacity: 0.4;
}

.pay-course__price span {
  font-family: Inter;
  font-size: 16px;
  font-weight: 700;
  line-height: 16px;
  letter-spacing: 0em;
  text-align: left;
  color: #0cba7a;
}
</style>
