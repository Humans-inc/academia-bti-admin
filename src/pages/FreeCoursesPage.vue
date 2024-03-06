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
          hint="Ссылка картинку, загруженную на GetCourse"
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
            label="Ссылка на картинку"
            variant="outlined"
            placeholder="https://..."
            hint="Ссылка картинку, загруженную на GetCourse"
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
      <div class="list__wrapper" id="draggableContainer">
        <div
          class="list-item"
          v-for="(item, index) in listData"
          :key="item.id"
          draggable="true"
          data-index="index"
        >
          <div class="list-item__name">{{ item.title }}</div>
          <div class="list-item__label">
            {{ item.label.text }}, {{ item.isForSkilled ? 'Для опытных' : 'Для новичков' }}
          </div>
          <div class="list-item__link">{{ item.description }}</div>
          <v-btn class="list-item__btn" variant="tonal" @click="handleRemoveItem(index)"
            >Удалить</v-btn
          >
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { freeCourses } from '../../public/data/freeCourses'

export default {
  data() {
    return {
      listData: [...freeCourses],
      formData: {
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
      }
    }
  },
  methods: {
    handleRemoveItem(index) {
      this.listData.splice(index, 1)
    },
    handleFormSubmitted() {
      if (this.formData.title.length) {
        const newData = {
          id: new Date().getTime(),
          ...this.formData
        }
        this.listData.unshift(newData)
        console.log(newData)
        // send to server
      } else {
        alert('Заполните все поля')
      }
    }
  }
}
document.addEventListener('DOMContentLoaded', () => {
  const container = document.getElementById('draggableContainer')
  let draggingElement = null

  container.addEventListener('dragstart', (e) => {
    draggingElement = e.target
    draggingElement.classList.add('dragging')
  })

  container.addEventListener('dragover', (e) => {
    e.preventDefault()
    const afterElement = getDragAfterElement(container, e.clientY)
    const currentElement = document.querySelector('.dragging')

    if (afterElement == null) {
      container.appendChild(currentElement)
    } else {
      container.insertBefore(currentElement, afterElement)
    }
  })

  container.addEventListener('dragend', () => {
    draggingElement.classList.remove('dragging')
    draggingElement = null
  })

  function getDragAfterElement(container, y) {
    const draggableElements = [...container.querySelectorAll('.list-item:not(.dragging)')]

    return draggableElements.reduce(
      (closest, child) => {
        const box = child.getBoundingClientRect()
        const offset = y - box.top - box.height / 2

        if (offset < 0 && offset > closest.offset) {
          return { offset, element: child }
        } else {
          return closest
        }
      },
      { offset: Number.NEGATIVE_INFINITY }
    ).element
  }
})
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
