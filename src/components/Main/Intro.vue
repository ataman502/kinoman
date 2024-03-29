<template>
  <section class="intro">
    <Loader v-if="loading" />
    <div
      v-else
      class="intro__image"
      :style="{ backgroundImage: 'url(' + introFilm.data.posterUrl + ')' }"
    >
      <div class="container-fluid">
        <div class="row">
          <div class="intro__descr">
            <div class="row justify-content-between align-items-center">
              <div class="col-lg-6  d-flex flex-column">
                <div class="intro__wrap">
                  <h2 class="intro__title">
                    {{ introFilm.data.nameRu }}
                  </h2>

                  <p class="intro__text">
                    {{ introFilm.data.description }}
                  </p>

                  <ul class="intro__descr-item">
                    <li>
                      Страна:
                      <span> {{ introFilm.data.countries }} </span>
                    </li>

                    <li>
                      Год:
                      <span>{{ introFilm.data.year }} </span>
                    </li>

                    <li>
                      Жанр:
                      <span> {{ introFilm.data.genres }} </span>
                    </li>

                    <li>
                      КиноПоиск:
                      <span class="intro__decr-rating">{{
                        introFilm.rating.rating
                      }}</span>
                    </li>
                  </ul>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <button
      class="btn btn--intro"
      @click="$router.push('/film/' + introFilm.data.filmId)"
    />
  </section>
</template>

<script>
import filterCountriesArr from '@/utils/filterCountries.js'
import filterGenresArr from '@/utils/filterGenres.js'

export default {
  name: 'Intro',
  data: () => ({
    introFilm: null,
    loading: true
  }),
  async mounted () {
    await this.getIntroFilm()
    this.filterGenres()
    this.filterCountries()

    this.loading = false
  },
  methods: {
    getIntroFilm () {
      this.introFilm = this.$store.state.intro.introFilm
    },

    async filterGenres () {
      const genres = await this.introFilm.data.genres
      this.introFilm.data.genres = filterGenresArr(genres)
    },
    async filterCountries () {
      const countries = await this.introFilm.data.countries
      this.introFilm.data.countries = filterCountriesArr(countries)
    }
  }
}
</script>

<style lang="scss">
@import '@/assets/style/vars/_vars';

.btn--intro {
  width: 100%;
  height: 100%;
  opacity: 0;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 5;
}

.bg-c {
  background-color: fade-out(#fff, 0.9%);
  padding-bottom: 20px;
}

.intro {
  margin: 0px;
  position: relative;
  width: 100%;
  height: 500px;
  overflow: hidden;
  // border-radius: 0 0  $border-radius__big $border-radius__big;
  box-shadow: $shadows__coords-x $shadows__coords-y $shadows__size + 3
    fade-out(#000, 0.7%);

  @media (max-width: $breackpoints__md) {
    height: 350px;
  }
}

.intro__image {
  padding-top: 80px;
  width: 100%;
  height: 100%;
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  background-size: cover;
  background-position: top;
  background-repeat: no-repeat;
  // background-attachment: fixed;
  // border-radius: 0 0 $border-radius__small $border-radius__small;

  &::after {
    content: '';
    opacity: 1;
    width: 100%;
    height: 100%;
    position: absolute;
    bottom: 0;
    background: linear-gradient(
      90deg,
      rgba(15, 15, 15, 1) 0%,
      rgba(15, 15, 15, 0.6) 50%,
      rgba(0, 0, 0, 0) 75%
    );

    @media (max-width: $breackpoints__md) {
      background: linear-gradient(
        to right,
        rgba(0, 0, 0, 0.9) 0%,
        rgba(4, 4, 4, 0.3) 50%,
        rgba(9, 9, 9, 0.9) 100%
      );
    }
  }
}

.intro__wrap {
  display: flex;
  flex-direction: column;
  padding: 0 10px;
}

.intro__descr {
  padding: 10px;
  width: 100%;
  position: relative;
  z-index: 2;
  padding: 10px 0;
  text-align: center;
}

.intro__title {
  font-family: $font-family__sans;
  @include adaptiv-font($size--large, $size--normal);
  font-weight: $font-weight__sans__light;
  line-height: $line-height--large - 12px;
  margin-bottom: 10px;
  letter-spacing: 2px;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;

  @media (max-width: $breackpoints__xs) {
    line-height: $line-height--normal;
  }
}

.intro__text {
  padding: 0 10px;
  display: -webkit-box;
  font-family: $font-family__sans;
  font-weight: $font-weight__sans__regular;
  line-height: $line-height--small + 5px;
  @include adaptiv-font($size--small + 10, $size--small);
  -webkit-line-clamp: 4;
  -webkit-box-orient: vertical;
  overflow: hidden;

  @media (max-width: $breackpoints__xs) {
    line-height: $line-height--small;
  }
}

.intro__descr-item {
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  text-align: center;
  max-width: 100%;
  padding: 10px;
  margin-top: 10px;
  @include adaptiv-font($size--normal, $size--small);

  li {
    font-family: $font-family__sans;
    font-weight: $font-weight__sans__light;
    max-width: 100%;
    overflow: hidden;
    text-overflow: ellipsis;

    span {
      font-family: $font-family__sans;
      font-weight: $font-weight__sans__regular;
    }

    &:last-child {
      font-size: $font-size--normal + 5px;
      font-weight: 600;
      color: #f60;
    }
  }
}

.intro__decr-rating {
  padding: 5px 10px;
  border-radius: $border-radius__small - 4;
}

@keyframes intro-scale {
  to {
    transform: scale(1);
  }
}

@keyframes opacity-animate {
  to {
    opacity: 1;
  }
}
</style>
