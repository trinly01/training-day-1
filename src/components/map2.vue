<template>
    <div style="width: 100% ; height: calc(100vh - 50px);">
      <v-map :zoom=13 :center="center">
        <v-tilelayer url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"></v-tilelayer>
        <v-marker :lat-lng="center" :icon="redMarker"></v-marker>
        <v-marker v-for="(b, i) in binays" :key="i" :lat-lng="b.coords" @l-click="showInfo(b)" :icon="binayMarkerIcon"></v-marker>
      </v-map>
    </div>
</template>

<script>
import { Map, TileLayer, Marker } from 'vue2-leaflet'
import BinayHead from 'src/statics/binay.png'
import iconRed from 'src/statics/marker.png'
import L from 'leaflet'

let binayMarkerIcon = L.icon({
  iconUrl: BinayHead,
  iconAnchor: [32, 32],
  iconSize: [64, 64]
})

let redMarker = L.icon({
  iconUrl: iconRed,
  iconAnchor: [16, 16],
  iconSize: [38, 38]
})

export default {
  mounted () {
    var options = {
      enableHighAccuracy: true,
      timeout: 5000,
      maximumAge: 0
    }

    let success = (pos) => {
      var crd = pos.coords

      console.log('Your current position is:')
      console.log(`Latitude : ${crd.latitude}`)
      console.log(`Longitude: ${crd.longitude}`)
      console.log(`More or less ${crd.accuracy} meters.`)

      this.center = [crd.latitude, crd.longitude]
    }

    function error (err) {
      console.warn(`ERROR(${err.code}): ${err.message}`)
    }

    navigator.geolocation.getCurrentPosition(success, error, options)
  },
  components: {
    'v-map': Map,
    'v-tilelayer': TileLayer,
    'v-marker': Marker
  },
  data () {
    return {
      binayMarkerIcon,
      redMarker,
      center: [47.413220, -1.219482],
      binays: [
        {
          name: 'Binaynay',
          coords: []
        },
        {
          name: 'Binay Air',
          coords: []
        },
        {
          name: 'Bimbi',
          coords: []
        }
      ]
    }
  }
}
</script>

<style>
  @import "~leaflet/dist/leaflet.css";
  /* @import "~leaflet-routing-machine/dist/leaflet-routing-machine.css"; */
</style>

