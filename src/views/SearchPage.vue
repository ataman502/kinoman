<template>
  <section class="searchpage">
    <div class="container-fluid">
      <div class="row justify-content-center">
        <input
          autofocus
          type="search"
          role="search"
          v-model="inputSearch"
          placeholder="Найдите свои любимые фильмы"
          @keypress.enter="searchFilms"
          tabindex="1"
          class="searchpage__input"
        >

        <ul class="searchpage__info row justify-content-arround">
          <FilmItemInfo
            class="col-xl-4 col-md-6"
            style="width: 100%;"
            v-for="film in search.films"
            :key="film.filmId"
            :item-info="film"
          />
        </ul>
      </div>

      <Loader v-if="loading" />
    </div>
  </section>
</template>

<script>
export default {
  name: 'SearchPage',
  data: () => ({
    inputSearch: '',
    search: {},

    loading: false
  }),
  methods: {
    async searchFilms () {
      this.loading = true
      this.search = await this.$store.dispatch('searchFilms', this.inputSearch)
      this.loading = false
    }
  }
}
</script>

<style lang="scss">
@import '@/assets/style/vars/_vars';

.searchpage {
  margin-top: $padding__views;
  width: 100%;
  height: 100%;
  padding: 0 10px;
  position: relative;
}

.searchpage__input {
  font-family: $font-family__sans;
  width: 90%;
  border: 0;
  display: block;
  margin: 20px 0;
  text-align: left;
  padding: 14px 10px;
  outline: none;
  border-radius: $border-radius__large;
  transition: $transition-duration $transition-timing-function;
  @include adaptiv-font($size--normal + 10, $size--normal);
}

.searchpage__info {
  border-radius: $border-radius__large;
  width: 100%;

  &::-webkit-scrollbar {
    width: 0;
    opacity: 0;
    visibility: hidden;
  }
}
</style>
