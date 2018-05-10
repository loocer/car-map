<template>
  <div class="hello">
    <div id="map" class="map"></div>
    jijij
  </div>
</template>

<script>
import Header from '@/components/Header'
export default {
  name: 'home',
  components: {
    'header-layer': Header
  },
  data () {
    return {
      msg: '哟嗬喂扎金花'
    }
  },
  mounted () {
    var view = new ol.View({
        center: [-9101767, 2822912],
        zoom: 14
      });

      var map = new ol.Map({
        controls: ol.control.defaults().extend([
          new ol.control.FullScreen()
        ]),
        layers: [
          new ol.layer.Tile({
            source: new ol.source.BingMaps({
              key: 'Your Bing Maps Key from http://www.bingmapsportal.com/ here',
              imagerySet: 'Aerial'
            })
          })
        ],
        target: 'map',
        view: view
      });
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
