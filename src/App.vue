<template>
  <div
    id="app"
    class="app"
    :class="`app--${theme.currentTheme}`"
  >
    <transition name="fade">
      <component :is="layout">
        <router-view />
      </component>
    </transition>
  </div>
</template>

<script>
import MainLayout from '@/layouts/MainLayout'
import EmptyLayout from '@/layouts/EmptyLayout'

import { mapGetters } from 'vuex'
export default {
  name: 'App',
  metaInfo () {
    return {
      title: this.$title(this.$route.meta.title)
    }
  },
  data: () => ({}),
  components: {
    MainLayout,
    EmptyLayout
  },
  computed: {
    ...mapGetters(['theme']),
    layout () {
      return (this.$route.meta.layout || 'main') + '-layout'
    }
  }
}
</script>

<style  lang="scss">
@import './assets/style/_base.scss';
@import './assets/style/manGrid.css';

.app {
  min-height: 100vh;
  position: relative;
  overflow-y: scroll;

  @media (max-width: $breackpoints__md) {
    padding-bottom: 50px;
  }
}

a {
  text-decoration: none;
}

// scrollbar
::-webkit-scrollbar {
  width: 10px;
  border-radius: $border-radius__large;

  &-track {
    background: rgba(59, 59, 59, 0);
  }

  &-thumb {
    background: rgba(134, 134, 134, 0.486);
    border-radius: $border-radius__large;
  }
}
</style>
