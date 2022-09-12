<template>
  <div class="weather-widget-container">
    <my-card>
      <div class="weather-widget-container__header">
        <weather-header
          :showSettings="store.isSettingsOpen"
          @click="toggleSettings"
        />
      </div>
      <settings v-if="store.isSettingsOpen" />
      <weather v-else />
      <div class="weather-widget__loader-wrapper">
        <my-loader
          v-show="store.isLoading"
          :visible="store.isLoading"
        />
      </div>
      <div class="weather-widget__error-wrapper">
        <error-component
          v-if="store.error !== null" :error="store.error ?? ''"
        />
      </div>
    </my-card>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { Settings, Weather } from '@/views';

import {
  WeatherHeader, ErrorComponent,
} from '@/components';
import { useWeatherStore } from '@/stores/weather';
import { saveToLocalStorage } from '@/helpers';
import { LS_WEATHER_STATE } from '@/constants';
import { MyCard, MyLoader } from '@/components/UI';

export default defineComponent({
  name: 'weather-widget',
  components: {
    Weather,
    Settings,
    WeatherHeader,
    ErrorComponent,
    MyCard,
    MyLoader,
  },
  setup() {
    const store = useWeatherStore();

    store.$subscribe((_, state) => {
      saveToLocalStorage(LS_WEATHER_STATE, state);
    });

    return { store };
  },
  methods: {
    toggleSettings() {
      this.store.toggleSettings();
      this.store.resetError();
      this.store.resetErrorAddCity();
    },
  },
});
</script>

<style lang="scss">
@import '@/styles/index.scss';
</style>