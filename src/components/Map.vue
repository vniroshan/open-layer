<template>
  <v-container>
    <v-card height="400" class="mx-auto">
      <div class="map" id="map" ref="map-root"></div>
      <div class="text-center pt-3">
        <v-btn color="primary" @click="myLocation()">Get your location</v-btn>
      </div>
      <div class="pt-3 text-center" v-if="coordinates">
        Your Location coordinates: {{ coordinates }}
      </div>
    </v-card>
  </v-container>
</template>

<script>
import Feature from "ol/Feature";
import Geolocation from "ol/Geolocation";
import Map from "ol/Map";
import Point from "ol/geom/Point";
import View from "ol/View";
import { Circle as CircleStyle, Fill, Stroke, Style } from "ol/style";
import { OSM, Vector as VectorSource } from "ol/source";
import { Tile as TileLayer, Vector as VectorLayer } from "ol/layer";
import "ol/ol.css";
export default {
  name: "HelloWorld",

  data: () => ({
    view: null,
    map: null,
    coordinates: 0,
  }),

  mounted() {
    this.view = new View({
      zoom: 2,
      center: [0, 0],
      constrainResolution: true,
    });
    this.map = new Map({
      target: "map",
      view: this.view,
      layers: [
        new TileLayer({
          source: new OSM(),
        }),
      ],
    });
  },
  methods: {
    myLocation() {
      const geolocation = new Geolocation({
        tracking: true,
        trackingOptions: {
          enableHighAccuracy: true,
        },
        projection: this.view.getProjection(),
      });
      geolocation.setTracking(true);
      
      const accuracyFeature = new Feature();
      geolocation.on("change:accuracyGeometry", function () {
        accuracyFeature.setGeometry(geolocation.getAccuracyGeometry());
      });

      const positionFeature = new Feature();
      positionFeature.setStyle(
        new Style({
          image: new CircleStyle({
            radius: 6,
            fill: new Fill({
              color: "#3399CC",
            }),
            stroke: new Stroke({
              color: "#fff",
              width: 2,
            }),
          }),
        })
      );
      setTimeout(() => {
        this.coordinates = geolocation.getPosition();
      }, 300);
      geolocation.on("change:position", function () {
        const coordinates = geolocation.getPosition();
        positionFeature.setGeometry(
          coordinates ? new Point(coordinates) : null
        );
      });
      new VectorLayer({
        map: this.map,
        source: new VectorSource({
          features: [accuracyFeature, positionFeature],
        }),
      });
    },
  },
};
</script>
<style>
.map {
  width: 100% !important;
  height: 100% !important;
  border: 2px solid black;
}
</style>
