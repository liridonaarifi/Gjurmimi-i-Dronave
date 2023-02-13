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
      zoom: 13,
      dronePos: [42.654753, 21.164133],
      droneIcon: L.icon({
        iconUrl: droneIcon,
        iconSize: [32, 32],
      }),
      pathCoords: [
        [42.654753, 21.164133],
        [42.653943, 21.163934],
      ],

      map: null,
      marker: null,
      index: 0,
      arr: [],
      DronIcon: null,
      array: [
        [
          [42.654584, 21.16407],
          [42.654279, 21.164043],
        ],
        [
          [42.654279, 21.164043],
          [42.653967, 21.164006],
        ],
        [
          [42.653967, 21.164006],
          [42.653536, 21.164006],
        ],
        [
          [42.653536, 21.164006],
          [42.653324, 21.164015],
        ],
        [
          [42.653324, 21.164015],
          [42.652873, 21.163862],
        ],
        [
          [42.652873, 21.163862],
          [42.652521, 21.163646],
        ],
        [
          [42.652521, 21.163646],
          [42.652209, 21.163537],
        ],
        [
          [42.652209, 21.163537],
          [42.651818, 21.163519],
        ],
        [
          [42.651818, 21.163519],
          [42.651565, 21.163573],
        ],
        [
          [42.651565, 21.163573],
          [42.651273, 21.163438],
        ],
        [
          [42.651273, 21.163438],
          [42.650762, 21.163691],
        ],
        [
          [42.650762, 21.163691],
          [42.650262, 21.163073],
        ],
        [
          [42.650262, 21.163073],
          [42.650252, 21.161286],
        ],
        [
          [42.650252, 21.161286],
          [42.651294, 21.161159],
        ],
      ],
    };
  },
  mounted() {
    this.map = L.map("map").setView([42.654584, 21.16407], 10);
    var OpenStreetMap_Mapnik = L.tileLayer(
      "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
      {
        maxZoom: 17,
        attribution:
          '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
      }
    ).addTo(this.map);

    this.DronIcon = L.icon({
      iconUrl: droneIcon,
      iconSize: [30, 30],

    });

    var latlngs = [
      [42.654584, 21.16407],
      [42.654584, 21.16407],
    ];
    this.marker = L.marker([42.654584, 21.16407], { icon: this.DronIcon }).addTo(
      this.map
    );
    var polyline = L.polyline(latlngs, { color: "red" }).addTo(this.map);
    this.updateDronePosition();
  },
  methods: {
    updateDronePosition(indexNumber) {
      let latlngs = [
        [42.654584, 21.16407],
        [42.654279, 21.164043],
        [42.653967, 21.164006],
        [42.653536, 21.164006],
        [42.653324, 21.164015],
        [42.652873, 21.163862],
        [42.652521, 21.163646],
        [42.652209, 21.163537],
        [42.651818, 21.163519],
        [42.651565, 21.163573],
        [42.651273, 21.163438],
        [42.650762, 21.163691],
        [42.650262, 21.163073],
        [42.650252, 21.161286],
        [42.651294, 21.161159],
      ];

      this.arr.push(latlngs[this.index]);
      this.marker.setLatLng(this.array[this.index][0]);
      var polyline = L.polyline(this.arr, { color: "red" }).addTo(this.map);
      // zoom the map to the polyline
      this.map.fitBounds(polyline.getBounds());
      this.index++;

      setTimeout(() => this.updateDronePosition(), 2000);
    },
  },
};
</script>

<style scoped></style>
