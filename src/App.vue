<script>
import ExpertsPage from './components/ExpertsPage.vue';
import FreeCoursesPage from './components/FreeCoursesPage.vue';
import VideoReviewsPage from './components/VideoReviewsPage.vue';
import SubcribeCoursesPage from './components/SubcribeCoursesPage.vue';
import CalendarEventsPage from './components/CalendarEventsPage.vue';
import MainSliderPage from './components/MainSliderPage.vue';
import MainSliderPagePic from './components/MainSliderPagePic.vue';
import CoursesPage from './components/CoursesPage.vue';

export default {
  components: {
    VideoReviewsPage,
    ExpertsPage,
    FreeCoursesPage,
    SubcribeCoursesPage,
    CalendarEventsPage,
    MainSliderPage,
    CoursesPage,
    MainSliderPagePic
  },
  data: () => ({
    tab: 'option-1'
  })
};
</script>

<template>
  <v-card>
    <div class="main-content">
      <v-tabs v-model="tab" color="primary" direction="vertical" class="main-content__nav">
        <v-tab value="option-1" class="main-content__nav-item"> Главный слайдер (картинками) </v-tab>
        <v-tab value="option-8" class="main-content__nav-item"> Главный слайдер </v-tab>
        <v-tab value="option-2" class="main-content__nav-item"> Календарь </v-tab>
        <v-tab value="option-3" class="main-content__nav-item"> Курсы </v-tab>
        <v-tab value="option-4" class="main-content__nav-item"> Бесплатные материалы </v-tab>
        <v-tab value="option-5" class="main-content__nav-item"> Эксперты компании </v-tab>
        <v-tab value="option-6" class="main-content__nav-item"> Видео-отзывы </v-tab>
        <v-tab value="option-7" class="main-content__nav-item"> Курсы абонемента </v-tab>
      </v-tabs>
      <v-window v-model="tab">
        <v-window-item value="option-1">
          <main-slider-page-pic />
        </v-window-item>
        <v-window-item value="option-8">
          <main-slider-page />
        </v-window-item>
        <v-window-item value="option-2">
          <calendar-events-page />
        </v-window-item>
        <v-window-item value="option-3">
          <courses-page />
        </v-window-item>
        <v-window-item value="option-4">
          <free-courses-page />
        </v-window-item>
        <v-window-item value="option-5">
          <experts-page />
        </v-window-item>
        <v-window-item value="option-6">
          <video-reviews-page />
        </v-window-item>
        <v-window-item value="option-7">
          <subcribe-courses-page />
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

  @media (max-width: 768px) {
    display: flex;
    flex-direction: column;
  }

  &__content {
    display: grid;
    grid-template-columns: repeat(2, 50%);

    @media (max-width: 1200px) {
      grid-template-columns: 1fr;
    }
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
