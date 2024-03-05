<script>
export default {
  props: {
    listTitle: String,
    listData: Array
  },
  methods: {
    removeItem(index) {
      this.$emit('remove-item', index)
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

<template>
  <div class="list">
    <h2 class="list__title">{{ listTitle }}</h2>
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
        <v-btn class="list-item__btn" variant="tonal" @click="removeItem(index)">Удалить</v-btn>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
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
