<script>
import VideoReviewsPage from './pages/VideoReviewsPage.vue'

export default {
  components: { VideoReviewsPage },
  data: () => ({
    tab: 'option-6'
  })
}
</script>

<template>
  <v-card>
    <div class="main-content">
      <v-tabs v-model="tab" color="primary" direction="vertical" class="main-content__nav">
        <v-tab value="option-1" class="main-content__nav-item"> Главный слайдер </v-tab>
        <v-tab value="option-2" class="main-content__nav-item"> Календарь </v-tab>
        <v-tab value="option-3" class="main-content__nav-item"> Курсы </v-tab>
        <v-tab value="option-4" class="main-content__nav-item"> Бесплатные Курсы </v-tab>
        <v-tab value="option-5" class="main-content__nav-item"> Эксперты компании </v-tab>
        <v-tab value="option-6" class="main-content__nav-item"> Видео-отзывы </v-tab>
        <v-tab value="option-7" class="main-content__nav-item"> Курсы абонемента </v-tab>
      </v-tabs>
      <v-window v-model="tab">
        <v-window-item value="option-1" class="main-content__content">
          Главный слайдер
        </v-window-item>
        <v-window-item value="option-2"> Календарь </v-window-item>
        <v-window-item value="option-3"> Курсы </v-window-item>
        <v-window-item value="option-4">
          Бесплатные Курсы
        </v-window-item>
        <v-window-item value="option-5">
          Эксперты компании
        </v-window-item>
        <v-window-item value="option-6">
          <video-reviews-page />
        </v-window-item>
        <v-window-item value="option-7">
          Курсы абонемента
        </v-window-item>
      </v-window>
    </div>
  </v-card>
</template>

<style scoped lang="scss">
.main-content {
  * {
    font-family: Arial, Helvetica, sans-serif;
  }

  display: grid;
  grid-template-columns: 250px 1fr;
  width: 100%;
  max-width: 1920px;
  min-height: 100vh;

  &__content {
    display: grid;
    grid-template-columns: repeat(2, 50%);
  }

  &__nav {
    padding: 20px;
    background: rgba(var(--v-theme-primary), 0.1);
  }

  &__nav-item {
    margin-bottom: 10px;
    background: #fff;
  }
}
</style>
