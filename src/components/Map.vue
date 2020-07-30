<template>
  <div class="Map">
    <div class="Map-Controls ">
      <h2>
        GMapsLocale, <i>however poorly named it maybe,</i> is a tool that let's
        you visualize the Location History that Google collects from you. To
        learn more about where to get this info, click
        <a href="https://google.com/takeout" target="_blank">here</a>. Upload
        your takeout to start visualizing.
      </h2>
      <h3 class="heading-1">
        Upload your Location History to begin visualizing.
      </h3>
      <input
        ref="file"
        type="file"
        id="file"
        @change="handleFileUpload()"
      /><br />
      <button class="upload-button" @click="submitFile()">
        Upload Location History</button
      ><br />
    </div>
    <div id="map" class="grid grid-content_wide"></div>
  </div>
</template>

<script>
import L from 'leaflet'
import HeatmapOverlay from 'leaflet-heatmap'
import axios from 'axios'

export default {
  data() {
    return {
      file: '',
      initCoords: {
        lat: 17.6801,
        long: 83.2016
      },
      map: null,
      gdata: {
        max: 8,
        data: []
      },
      heatmapLayer: null
    }
  },
  methods: {
    initMap() {
      var tileLayer = L.tileLayer(
        'https://cartodb-basemaps-{s}.global.ssl.fastly.net/rastertiles/voyager/{z}/{x}/{y}.png',
        {
          maxZoom: 18,
          attribution:
            '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>, &copy; <a href="https://carto.com/attribution">CARTO</a>'
        }
      )

      var cfg = {
        // radius should be small ONLY if scaleRadius is true (or small radius is intended)
        // if scaleRadius is false it will be the constant radius used in pixels
        radius: 10,
        maxOpacity: 0.8,
        // scales the radius based on map zoom
        // if set to false the heatmap uses the global maximum for colorization
        // if activated: uses the data maximum within the current map boundaries
        //   (there will always be a red spot with useLocalExtremas true)
        useLocalExtrema: true,
        // which field name in your data represents the latitude - default "lat"
        latField: 'lat',
        // which field name in your data represents the longitude - default "lng"
        lngField: 'lng',
        // which field name in your data represents the data value - default "value"
        valueField: 'count'
      }

      // console.log(HeatmapOverlay)
      this.heatmapLayer = new HeatmapOverlay(cfg)

      this.map = L.map('map', {
        center: new L.LatLng(this.initCoords.lat, this.initCoords.long),
        zoom: 8,
        layers: [tileLayer, this.heatmapLayer]
      })
    },

    initLayers() {
      this.gdata.data = this.gdata.data.map(feature => ({
        lat: feature.latitudeE7 / Math.pow(10, 7),
        lng: feature.longitudeE7 / Math.pow(10, 7),
        count: 1
      }))
      console.log('initLayers', this.gdata.data.length)
      this.heatmapLayer.setData(this.gdata)
    },
    getLocation() {
      let vm = this
      axios
        .get('http://ip-api.com/json')
        .then(res => {
          vm.initCoords.lat = res.data.lat
          vm.initCoords.long = res.data.lon
          this.initMap()
        })
        .catch(err => {
          // TODO: handle not being able to get Location
          console.log(err)
          this.initMap()
        })
    },
    handleFileUpload() {
      this.file = this.$refs.file.files[0]
    },
    submitFile() {
      // TODO: handle if incorrect format file was uploaded
      let vm = this
      let read = new FileReader()
      read.readAsBinaryString(this.file)
      read.onloadend = () => {
        vm.gdata.data = JSON.parse(String(read.result)).locations
        vm.initLayers()
      }
    }
  },
  mounted() {
    this.getLocation()
  }
}
</script>

<style scoped>
a {
  color: rgb(42, 205, 255);
}

.Map {
  display: flex;
  flex-direction: column;
  align-items: center;
  border-top: 0.6rem solid #ff3e00;
}

#map {
  height: 600px;
  width: 100%;
}

#file {
  margin: 1rem 0;
  color: #fff;
}
.Map-Controls {
  padding: 2em;
  width: 70%;
  max-width: 300rem;
}

.Map-Controls h2 {
  font-weight: 100;
  font-family: 'Barlow Condensed', sans-serif;
  font-size: 2.3rem;
  color: #fff;
}

.heading-1 {
  font-size: 1.5em;
  font-family: 'Overpass', sans-serif;
  color: #444;
  font-weight: normal;
  margin: 2rem 0;
  color: #999;
}

.upload-button {
  background-color: #ff3e00;
  border: none;
  min-width: 200px;
  padding: 0.75em 1em;
  color: #fff;
  border-radius: 5px;
  cursor: pointer;
  margin: 1em 0;
  font-size: 1.5em;
  font-family: 'Overpass', sans-serif;
}
.upload-button:hover {
  background-color: #f05a28;
}
</style>
