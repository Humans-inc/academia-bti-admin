<template>
  <div class="main-content__content">
    <div class="form">
      <h2 class="form__title">Добавить</h2>
      <form class="form__wrapper" @submit.prevent="handleFormSubmitted">
        <v-text-field
          label="Ссылка на картинку"
          variant="outlined"
          placeholder="https://..."
          v-model="formData.bgImage"
        ></v-text-field>
        <v-btn size="x-large" variant="tonal" class="v-btn text-primary" type="submit"
          >Добавить</v-btn
        >
      </form>
    </div>
    <div class="list">
      <h2 class="list__title">Слайдер</h2>
      <draggable v-model="listData" class="list__wrapper" item-key="video">
        <template #item="{ element }">
          <div class="list-item" itemKey="element.id">
            <img :src="element.bgImage" alt="" />
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
  bgImage: ''
});

const handleFormSubmitted = () => {
  if (formData.value.bgImage.length) {
    const newData = {
      bgImage: formData.value.bgImage,
    };
    listData.value.unshift(newData);
    formData.value = {
      bgImage: '',
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
