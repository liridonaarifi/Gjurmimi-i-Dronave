<template>
  <div style="height: 100vh; width: 100vw">
    <div ref="map" id="map" style="height: 100vh; width: 100vw"></div>
  </div>
</template>

<script>
import "leaflet/dist/leaflet.css";
import L from "leaflet";
import { LMap, LTileLayer, LMarker, LPolyline } from "@vue-leaflet/vue-leaflet";
import droneIcon from "../assets/drone.svg";
import { initialCoords, latAndLongs, path } from "@/services/locations";

export default {
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPolyline,
  },
  data() {
    return {
      center: [42.654753, 21.164133],
      droneIcon: L.icon({
        iconUrl: droneIcon,
        iconSize: [32, 32],
      }),

      map: null,
      marker: null,
      index: 0,
      arr: [],
      path: path,
    };
  },
  mounted() {
    this.map = L.map("map").setView(latAndLongs[0], 17);
    const tileLayer = L.tileLayer(
        "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
        {
          maxZoom: 17,
          attribution:
              '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
        }
    ).addTo(this.map);

    this.marker = L.marker(latAndLongs[0], { icon: this.droneIcon }).addTo(this.map);
    this.polyline = L.polyline(initialCoords, { color: "red" }).addTo(this.map);
    this.bounds = this.polyline.getBounds();
    this.updateDronePosition();
  },
  methods: {
    updateDronePosition() {
      this.arr.push(latAndLongs[this.index]);
      this.marker.setLatLng(this.path[this.index][0]);
      this.polyline.setLatLngs(this.arr);

      const newBounds = this.polyline.getBounds();
      if (!this.bounds.equals(newBounds)) {
        this.bounds = newBounds;
        this.map.fitBounds(this.bounds);
      }

      this.index++;

      setTimeout(() => this.updateDronePosition(), 2000);
    }
  }
};
</script>

<style scoped></style>
