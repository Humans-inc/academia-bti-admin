<template>
  <div class="main-content__content">
    <video-review-form />
    <items-list :listTitle="listTitle" :listData="listData" @remove-item="handleRemoveItem" />
  </div>
</template>

<script>
import { ref } from 'vue'
import VideoReviewForm from '../components/VideoReviewForm.vue'
import { videoReviews } from '../../public/data/video-reviews'
import ItemsList from '../components/ItemsList.vue'

export default {
  components: { ItemsList, VideoReviewForm },
  setup() {
    const listTitle = ref('Видео-отзывы')

    return {
      listTitle
    }
  },
  data() {
    return {
      listData: [...videoReviews]
    }
  },
  methods: {
    handleRemoveItem(index) {
      this.listData.splice(index, 1)
    }
  }
}
</script>

<style lang="scss" scoped></style>
