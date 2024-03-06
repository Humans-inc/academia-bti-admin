<template>
  <div class="main-content__content">
    <div class="form">
      <h2 class="form__title">Добавить отзыв</h2>
      <form class="form__wrapper" @submit.prevent="handleFormSubmitted">
        <v-text-field
          label="Имя ученика"
          variant="outlined"
          placeholder="Сергей Петрович"
          v-model="formData.name"
        ></v-text-field>
        <v-text-field
          label="Ссылка на отзыв"
          variant="outlined"
          placeholder="https://www.youtube.com/embed/"
          hint="Ссылка на Youtube или Vimeo"
          v-model="formData.link"
        ></v-text-field>
        <v-btn size="x-large" variant="tonal" class="v-btn text-primary" type="submit"
          >Добавить</v-btn
        >
      </form>
    </div>
    <div class="list">
      <h2 class="list__title">Видео-отзывы</h2>
      <div class="list__wrapper" id="draggableContainer">
        <div
          class="list-item"
          v-for="(item, index) in listData"
          :key="item.id"
          draggable="true"
          data-index="index"
        >
          <div class="list-item__name">{{ item.name }}</div>
          <div class="list-item__link">{{ item.link }}</div>
          <v-btn class="list-item__btn" variant="tonal" @click="handleRemoveItem(index)"
            >Удалить</v-btn
          >
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { videoReviews } from '../../public/data/video-reviews'

export default {
  data() {
    return {
      listData: [...videoReviews],
      formData: {
        name: '',
        link: ''
      }
    }
  },
  methods: {
    handleRemoveItem(index) {
      this.listData.splice(index, 1)
    },
    handleFormSubmitted() {
      if (this.formData.name.length && this.formData.link.length) {
        const newData = {
          id: new Date().getTime(),
          name: this.formData.name,
          link: this.formData.link
        }
        this.listData.unshift(newData);
        this.formData.name = '';
        this.formData.link = '';
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
</style>
