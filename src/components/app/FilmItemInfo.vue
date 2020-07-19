<template>
  <router-link
    tag="a"
    :to="'/film/' + itemInfo.filmId"
    class="filmitem col-4"
  >
    <div class="filmitem__img">
      <img
        :src="itemInfo.posterUrl"
        alt=""
        class="img"
      >
    </div>

    <div class="filmitem__content">
      <div class="filmitem__text">
        <div class="filmitem__title">
          <h3>{{ itemInfo.nameRu }}</h3>
        </div>
        <ul class="filmitem__ul">
          <li>{{ itemInfo.releaseDate }}</li>
          <li>{{ filterGenres }}</li>
          <li>{{ filterCountries }}</li>
          <li>{{ itemInfo.duration }} минут</li>
        </ul>
      </div>

      <div class="filmitem__rating">
        <h5 :class="rating">
          {{ itemInfo.rating }}
        </h5>
      </div>

      <button class="filmitem__menu">
        <svg viewBox="0 0 515.555 515.555">
          <path
            fill="currentColor"
            d="m303.347 18.875c25.167 25.167 25.167 65.971 0 91.138s-65.971 25.167-91.138 0-25.167-65.971 0-91.138c25.166-25.167 65.97-25.167 91.138 0"
          /><path
            fill="currentColor"
            d="m303.347 212.209c25.167 25.167 25.167 65.971 0 91.138s-65.971 25.167-91.138 0-25.167-65.971 0-91.138c25.166-25.167 65.97-25.167 91.138 0"
          /><path
            fill="currentColor"
            d="m303.347 405.541c25.167 25.167 25.167 65.971 0 91.138s-65.971 25.167-91.138 0-25.167-65.971 0-91.138c25.166-25.167 65.97-25.167 91.138 0"
          />
        </svg>
      </button>
    </div>
  </router-link>
</template>

<script>
import filterCountriesArr from '@/utils/filterCountries.js'
import filterGenresArr from '@/utils/filterGenres.js'

export default {
  name: 'FilmItemInfo',
  props: {
    itemInfo: {
      type: Object,
      required: true
    }
  },
  computed: {
    rating () {
      if (this.itemInfo.rating >= 6.5) {
        return 'green'
      } else if (this.itemInfo.rating >= 4.1) {
        return 'yellow'
      } else {
        return 'red'
      }
    },
    filterGenres () {
      const genres = this.itemInfo.genres
      return filterGenresArr(genres)
    },
    filterCountries () {
      const countries = this.itemInfo.countries
      return filterCountriesArr(countries)
    }
  }
}
</script>

<style lang="less">
@import "@/assets/style/vars/vars.module";

.filmitem__link {
  max-width: 500px;
  &:hover,
  &:focus,
  &:active {
    .filmitem {
      background-color: fade(#fff, 10%);
      color: @colors__green;
    }
  }
}

.filmitem {
  max-width: 450px;
  width: 100%;
  padding: 0 12px;
  background-color: @colors__blackPrimary;
  overflow: hidden;
  display: flex;
  position: relative;
  margin-right: 20px;
  margin-bottom: 20px;
  transition: @transition-duration @transition-timing-function;
  user-select: none;
  border-radius: @border-radius__small - 5;

  &:hover,
  &:focus,
  &:active {
    background-color: fade(#fff, 10%);
    color: @colors__green;
    cursor: pointer;
  }
}

.filmitem__img {
  max-width: 185px;
  height: 100%;
  max-height: 230px;
  margin-right: 10px;

  img {
    height: 100%;
  }
}

.filmitem__content {
  padding: 10px;
  overflow: hidden;
}

.filmitem__title {
  margin-bottom: 20px;
  max-width: 220px;
  display: flex;

  h3 {
    font-size: @font-size--normal + 10;
    font-family: @font-family__sans__black;
    font-weight: 900;
    line-height: @line-height--small + 5;
    color: @colors__grays--lighter;
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
  }
}

.filmitem__ul {
  max-width: 220px;
  li {
    font-size: @font-size--normal;
    line-height: @line-height--normal;
    font-weight: 400;
    color: @colors__grays;
    margin-bottom: 5px;
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;

    &:first-child {
      font-weight: 300;
      font-family: @font-family__sans__light;
    }
  }
}

.filmitem__rating {
  position: absolute;
  right: 10px;
  bottom: 20px;

  h5 {
    font-size: @font-size--large - 10;
    line-height: @line-height--normal;
    font-weight: 400;
    color: @colors__green;

    &.green {
      color: @colors__green;
    }

    &.yellow {
      color: @colors__yellow;
    }

    &.red {
      color: @colors__red;
    }
  }
}

.filmitem__menu {
  padding: 5px;
  position: absolute;
  right: 0px;
  top: 20px;
  width: 60px;
  color: @colors__grays;
  border-radius: @border-radius__small;
  transition: @transition-duration @transition-timing-function;

  &:hover,
  &:focus,
  &:active {
    background-color: fade(#fff, 10%);
    color: @colors__green;
  }
}
</style>