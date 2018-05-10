<template>
  <div class="hello">
    <div id="map" class="map"><div id="popup"></div></div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'login',
  data () {
    return {
      msg: '哟嗬喂扎金花',
      name: '1',
      password: '1qaz'
    }
  },
  methods: {
    login(){
      var iconFeature = new ol.Feature({
        geometry: new ol.geom.Point([0, 0]),
        name: 'Null Island',
        population: 4000,
        rainfall: 500
      });

      var iconStyle = new ol.style.Style({
        image: new ol.style.Icon(/** @type {olx.style.IconOptions} */ ({
          anchor: [0.5, 46],
          anchorXUnits: 'fraction',
          anchorYUnits: 'pixels',
          src: 'https://openlayers.org/en/v4.6.5/examples/data/icon.png'
        }))
      });

      iconFeature.setStyle(iconStyle);

      var vectorSource = new ol.source.Vector({
        features: [iconFeature]
      });

      var vectorLayer = new ol.layer.Vector({
        source: vectorSource
      });

      var rasterLayer = new ol.layer.Tile({
        source: new ol.source.BingMaps({
          key: 'Your Bing Maps Key from http://www.bingmapsportal.com/ here',
          imagerySet: 'Aerial'
        })
      });

      var map = new ol.Map({
        layers: [rasterLayer, vectorLayer],
        controls: ol.control.defaults().extend([
          new ol.control.FullScreen()
        ]),
        target: document.getElementById('map'),
        view: new ol.View({
          center: [-9101767, 2822912],
          zoom: 14
        })
      });

      var element = document.getElementById('popup');

      var popup = new ol.Overlay({
        element: element,
        positioning: 'bottom-center',
        stopEvent: false,
        offset: [0, -50]
      });
      map.addOverlay(popup);

      // display popup on click
      map.on('click', function(evt) {
        var feature = map.forEachFeatureAtPixel(evt.pixel,
            function(feature) {
              return feature;
            });
        if (feature) {
          var coordinates = feature.getGeometry().getCoordinates();
          popup.setPosition(coordinates);
          $(element).popover({
            'placement': 'top',
            'html': true,
            'content': feature.get('name')
          });
          $(element).popover('show');
        } else {
          $(element).popover('destroy');
        }
      });

      // change mouse cursor when over marker
      map.on('pointermove', function(e) {
        if (e.dragging) {
          $(element).popover('destroy');
          return;
        }
        var pixel = map.getEventPixel(e.originalEvent);
        var hit = map.hasFeatureAtPixel(pixel);
        map.getTarget().style.cursor = hit ? 'pointer' : '';
      });
    }
  },
  mounted () {
    this.login()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
  .map {
    height: 100%;
    width: 100%;
  }
</style>
