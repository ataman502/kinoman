<template>
  <div class="release">
    <Loader v-if="loading" />

    <hooper
      v-else
      :settings="hooperSettings"
    >
      <slide
        v-for="film in charts.films"
        :key="film.filmId"
        class="films"
      >
        <FilmItemInfo
          :item-info="film"
          class="film"
        />
      </slide>
      <!-- <hooper-navigation slot="hooper-addons" /> -->
    </hooper>
  </div>
</template>

<script>
import { Hooper, Slide } from 'hooper'
import 'hooper/dist/hooper.css'

export default {
  name: 'Charts',
  data: () => ({
    charts: {},
    loading: true,

    hooperSettings: {
      itemsToShow: 3.1,
      mouseDrag: false,
      trimWhiteSpace: true,
      autoPlay: true,
      playSpeed: 10000,
      hoverPause: true,
      breakpoints: {
        1303: {
          itemsToShow: 3.5
        },
        993: {
          itemsToShow: 2.1
        },
        645: {
          itemsToShow: 2.1
        },
        0: {
          itemsToShow: 1.1
        }
      }
    }
  }),
  computed: {
    chartLS () {
      return localStorage.charts
    }
  },
  components: {
    Hooper,
    Slide
    // HooperNavigation
  },
  async created () {
    this.$store.state.app.loading = true

    await this.getChartsFilms()
    this.$store.state.app.loading = false
    this.loading = false
  },
  methods: {
    getLocalStorageInfo () {
      this.getChartLS()
    },
    getChartLS () {
      // Получение фильмов из LS
      if (!this.chartLS) {
        return
      }

      this.charts = JSON.parse(localStorage.getItem('charts'))
    },
    setChartLS () {
      // Отправляем информацию о фильмах в LS
      const charts = JSON.stringify(this.charts)
      localStorage.setItem('charts', charts)
    },

    async getChartsFilms () {
      try {
        this.charts = await this.$store.dispatch('getChartFilms')
        this.setChartLS()
      } catch (e) {
        this.getChartLS()
      }
    }
  }
}
</script>

<style scoped lang="scss">
@import '@/assets/style/vars/_vars';

.release {
  overflow: hidden;
  position: relative;

  &::after {
    content: '';
    opacity: 1;
    width: 10%;
    height: 100%;
    position: absolute;
    bottom: 0;
    right: 0;
    background: linear-gradient(
      to left,
      rgba(0, 0, 0, 0.5) 0%,
      rgba(0, 0, 0, 0.4) 50%,
      rgba(9, 9, 9, 0) 100%
    );
  }

  ::v-deep .hooper {
    height: auto;
  }
}

.film {
  width: 100%;
  margin: 0;

  .filmitem {
    margin: 0;
  }
}
</style>
