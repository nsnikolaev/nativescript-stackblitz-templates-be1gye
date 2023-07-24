<template>
  <Page>
    <ActionBar>
      <Label text="Home" class="font-bold text-lg" />
    </ActionBar>

    <GridLayout>
      <Label
        class="text-xl align-middle text-center text-gray-500"
        :text="message"
      />
    </GridLayout>
  </Page>
</template>

<script lang="ts">
import Vue from 'nativescript-vue';

import * as geolocation from '@nativescript/geolocation';
import { CoreTypes } from '@nativescript/core';

export default Vue.extend({
  data() {
    return {
      latitude: null,
      longitude: null,
    };
  },
  computed: {
    message() {
      if (!this.latitude || !this.longitude) {
        return 'Waiting geolocation...';
      } else {
        return `Location: { ${this.latitude}, ${this.longitude}}`;
      }
    },
  },
  methods: {},
  async mounted() {
    await geolocation.enableLocationRequest();
    geolocation
      .getCurrentLocation({
        desiredAccuracy: CoreTypes.Accuracy.high,
        maximumAge: 5000,
        timeout: 20000,
      })
      .then((currentLocation) => {
        this.latitude = currentLocation.latitude;
        this.longitude = currentLocation.longitude;
      });
  },
});
</script>

<style>
/* .info {
    font-size: 20;
  } */
</style>
