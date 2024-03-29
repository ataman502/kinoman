<template>
  <section class="items collection">
    <Loader v-if="loading" />

    <transition
      v-else
      name="transform-fade"
    >
      <div class="container-fluid">
        <div class="items__text">
          <h4>Релизы за {{ currentMonth().ru }}</h4>
        </div>

        <transition-group
          class="row justify-content-arround "
          tag="ul"
          name="transform-fade"
        >
          <FilmItemInfo
            class="col-xl-4 col-md-6"
            v-for="release in newRelease.releases"
            :key="release.filmId"
            :item-info="release"
          />
        </transition-group>

        <Loader v-if="lazyLoading" />
      </div>
    </transition>

    <span
      class="more"
      ref="more"
    />
  </section>
</template>

<script>
import FilmItemInfo from '@/components/app/FilmItemInfo'

export default {
  name: 'NewRelease',
  data: () => ({
    newRelease: {},
    page: 1,
    totalMovies: 10,

    date: {
      full: null,
      month: 'Januar',
      year: '2021'
    },

    loading: true,
    lazyLoading: false
  }),
  computed: {
    checkingItemsRelease () {
      const filmsArr = Object.keys(this.newRelease.releases)
      const totalMovies = this.newRelease.total

      return filmsArr.length >= totalMovies
    },
    releasesLS () {
      return JSON.parse(localStorage.getItem('releases'))
    }
  },
  async created () {
    this.$store.state.app.loading = true

    if (this.releasesLS) {
      this.getNewReleaseLS()
      this.loading = false
    }

    await this.getDate()
    await this.getNewRelease()
    this.$store.state.app.loading = false
    this.loading = false
  },
  async mounted () {
    this.lazyLoad()
  },
  components: {
    FilmItemInfo
  },
  methods: {
    getNewReleaseLS () {
      if (!this.releasesLS) {
        return
      }

      this.newRelease = this.releasesLS
    },
    setNewReleaseLS () {
      // Отправляем информацию о фильме в LS
      const newRelease = JSON.stringify(this.newRelease)
      localStorage.setItem('releases', newRelease)
    },

    async getDate () {
      this.date.full = new Date()
      this.date.year = await this.currentYear()
      this.date.month = await this.currentMonth().en
    },

    async getNewRelease () {
      const options = {
        year: this.date.year,
        month: this.date.month,
        page: 1
      }

      try {
        this.newRelease = await this.$store.dispatch('getNewRelease', options)
        this.setNewReleaseLS()
      } catch (e) {}
    },

    currentMonth () {
      const date = this.date.full
      const options = {
        month: 'long'
      }

      if (this.page > 2 && this.checkingItemsRelease) {
        const currentMonth = date.getMonth()
        date.setMonth(currentMonth - 1)
        this.page = 0
      }

      return {
        en: new Intl.DateTimeFormat('en-EN', options).format(date),
        ru: new Intl.DateTimeFormat('ru-RU', options).format(date)
      }
    },
    currentYear () {
      const date = this.date.full
      const options = {
        year: 'numeric'
      }
      return new Intl.DateTimeFormat('en-EN', options).format(date)
    },

    // Загрузить больше
    async loadMore () {
      this.lazyLoading = true

      await this.getMoreNewRelease()

      this.lazyLoading = false
    },

    async getMoreNewRelease () {
      // Если закончились релизы в текущем месяцу, переключать на другой
      if (this.checkingItemsRelease) {
        this.date.month = await this.currentMonth().en
      }

      this.page++

      const options = {
        year: this.date.year,
        month: this.date.month,
        page: this.page
      }

      try {
        const newRelease = await this.$store.dispatch('getNewRelease', options)

        newRelease.releases.forEach(film => {
          this.newRelease.releases.push(film)
        })
      } catch (e) {
        console.log(e)
      }
    },
    async lazyLoad () {
      const options = {
        root: null,
        threshold: 0
      }

      const observer = new IntersectionObserver((entries, observer) => {
        entries.forEach(entry => {
          if (entry.isIntersecting) {
            if (!this.loading) {
              this.loadMore()
            }
            observer.unobserve()
          }
        })
      }, options)

      const el = this.$refs.more
      observer.observe(el)
    }
  }
}
</script>

<style lang="scss">
@import '@/assets/style/vars/_vars';

.collection {
  padding-top: $padding__views - 40;
}

.items__text {
  margin-bottom: 20px;
  h4 {
    font-family: $font-family__sans;
    line-height: $line-height--large;
    @include adaptiv-font($size--large + 10, $size--normal + 10);
  }
}

.more {
  display: block;
  height: 5px;
  opacity: 0;
}
</style>
