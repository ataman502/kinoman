<template>
  <div class="release">
    <Loader v-if="loading" />

    <hooper
      v-else
      :settings="hooperSettings"
    >
      <slide
        v-for="release in newRelease"
        :key="release.filmId"
      >
        <FilmItemInfo
          :item-info="release"
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
  name: 'NewRelease',
  data: () => ({
    newRelease: {},
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
    newReleaseLS () {
      return JSON.parse(localStorage.getItem('newRelease'))
    }
  },
  components: {
    Hooper,
    Slide
    // HooperNavigation
  },
  async created () {
    this.$store.state.app.loading = true

    // Если ничего нет в LS
    if (this.newReleaseLS) {
      this.getLocalStorageInfo()
      this.loading = false
    }

    await this.getNewRelease()
    this.$store.state.app.loading = false
    this.loading = false
  },
  methods: {
    getLocalStorageInfo () {
      this.getNewReleaseLS()
    },
    getNewReleaseLS () {
      // Получение фильмов из LS
      if (!this.newReleaseLS) {
        return
      }

      this.newRelease = JSON.parse(localStorage.getItem('newRelease'))
    },
    setNewReleaseLS () {
      // Отправляем информацию о фильмах в LS
      const newRelease = JSON.stringify(this.newRelease)
      localStorage.setItem('newRelease', newRelease)
    },

    async getNewRelease () {
      const currentDate = new Date()
      const currentYear = await this.currentYear(currentDate)

      const options = {
        rating: {
          from: '0',
          to: '10'
        },
        year: {
          from: currentYear,
          to: currentYear
        },
        page: [3, 4, 1, 2, 5]
      }

      try {
        this.newRelease = await this.$store.dispatch('getReleasesArr', options)
        this.setNewReleaseLS()
      } catch (e) {
        this.getNewReleaseLS()
      }
    },
    currentYear (date) {
      const options = {
        year: 'numeric'
      }
      return new Intl.DateTimeFormat('en-EN', options).format(date)
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
