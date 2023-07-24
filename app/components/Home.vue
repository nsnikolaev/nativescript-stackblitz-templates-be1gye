<template>
  <Page>
    <ActionBar>
      <Label text="Home" class="font-bold text-lg" />
    </ActionBar>

    <FlexboxLayout flexDirection="column">
      <Label>
        {{ message }}
      </Label>
      <Label height="90%" backgroundColor="#ff9900">
        <MapView @ready="onMapReady" />
      </Label>
    </FlexboxLayout>
  </Page>
</template>

<script lang="ts">
import Vue from 'nativescript-vue';
import { CoreTypes } from '@nativescript/core';
import * as geolocation from '@nativescript/geolocation';

import { MapView } from '@nativescript/google-maps/vue';

export default Vue.extend({
  data() {
    return {
      hasLocation: false,

      latitude: null,
      longitude: null,
    };
  },
  components: {
    MapView,
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
  methods: {
    onMapReady(event: MapReadyEvent) {
      const map: GoogleMap = event.map;
      console.log('Map is ready!', map);
    },
  },
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
