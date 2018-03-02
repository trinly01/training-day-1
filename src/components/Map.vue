<style>
  input[value$="a"] {
    background-color: red;
  }
</style>


<template>
  <div style="width: 1024px; height: 768px;">
    <input type="password" v-model="word" value="a" />
    <v-map ref="vmaaaaaaaAp" :zoom=13 :center="center">
      <v-tilelayer url="http://{s}.tile.osm.org/{z}/{x}/{y}.png"></v-tilelayer>
      <v-marker @l-click="showInfo" :icon="redMarker" :lat-lng="[14.585669, 121.051417]"></v-marker>
    </v-map>
  </div>
  
</template>

<script>
import { Map, TileLayer, Marker } from 'vue2-leaflet'
import L from 'leaflet'
import iconRed from 'src/statics/marker.png'

import 'leaflet-routing-machine'

let redMarker = L.icon({
  iconUrl: iconRed,
  iconAnchor: [16, 16],
  iconSize: [38, 38]
})

export default {
  components: {
    'v-map': Map,
    'v-tilelayer': TileLayer,
    'v-marker': Marker
  },
  mounted () {
    L.Routing.control({
      waypoints: [
        L.latLng(this.center[0], this.center[1]),
        L.latLng(14.489541, 121.052673)
      ]
    }).addTo(this.$refs.vmaaaaaaaAp.mapObject)

    // console.log(routings.getWaypoints())
  },
  data () {
    return {
      word: '',
      redMarker,
      center: [14.585669, 121.051417],
      marker: []
    }
  },
  methods: {
    showInfo () {
      alert('Hello')

      function success (pos) {
        var crd = pos.coords

        console.log('Your current position is:')
        console.log('Latitude : ' + crd.latitude)
        console.log(`Longitude: ${crd.longitude}`)
        console.log(`More or less ${crd.accuracy} meters.`)
      };

      function error (err) {
        console.warn(`ERROR(${err.code}): ${err.message}`)
      }

      navigator.geolocation.getCurrentPosition(success, error)
    }
  }
}
</script>

<style>
  @import "~leaflet/dist/leaflet.css";
  @import "~leaflet-routing-machine/dist/leaflet-routing-machine.css";
</style>



