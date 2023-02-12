<template>
  <div style="height: 100vh; width: 100vw">
    <l-map ref="map" v-model:zoom="zoom" :center="dronePos">
      <l-tile-layer
          url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
          layer-type="base"
          name="OpenStreetMap"
      ></l-tile-layer>
      <l-polyline v-if="$refs.map" ref="path" :lat-lngs="pathCoords"></l-polyline>
      <l-marker v-if="$refs.map" ref="marker" :icon="droneIcon" :lat-lng="dronePos"></l-marker>
    </l-map>
  </div>
</template>

<script>
import "leaflet/dist/leaflet.css";
import L from "leaflet";
import { LMap, LTileLayer, LMarker, LPolyline } from "@vue-leaflet/vue-leaflet";
import droneIcon from "../assets/drone.svg"

export default {
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPolyline
  },
  data() {
    return {
      center: [47.41322, -1.219482],
      zoom: 16,
      dronePos: [47.41322, -1.219482],
      droneIcon: L.icon({
        iconUrl: droneIcon,
        iconSize: [32, 32]
      }),
      pathCoords: [[47.41322, -1.219482]]
    };
  },
  mounted() {
    this.updateDronePosition();
  },
  methods: {
    updateDronePosition() {
      this.dronePos = [
        this.dronePos[0] + Math.random() * 0.0002 - 0.0001,
        this.dronePos[1] + Math.random() * 0.0002 - 0.0001
      ];
      try {
        debugger
        this.pathCoords.push(this.dronePos);
      } catch (error) {
        console.error("Error updating path coordinates: ", error);
      }
      setTimeout(() => this.updateDronePosition(), 2000);
    }
  }

};
</script>

<style scoped></style>
