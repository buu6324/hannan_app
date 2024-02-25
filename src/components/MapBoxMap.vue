<template>
  <div id="map" style="width: 100%; height: 400px;"></div>
</template>

<script>
import mapboxgl from 'mapbox-gl';


export default {
  name: 'MapBoxMap',
  mounted() {
    mapboxgl.accessToken = 'pk.eyJ1IjoibWFwYm94MjAyNCIsImEiOiJjbHNzeTI3Y3AxcTd5MmxueW5pNm50bjVxIn0.o7TrLJvB8eiqONQSrEPMOQ';
    const map = new mapboxgl.Map({
      container: 'map',
      style: 'mapbox://styles/mapbox/streets-v11',
      center: [113.837701458, 30.2430320975],
      zoom: 9
    });

    // 添加GeoServer图层的代码
    map.on('load', function() {
      map.addLayer({
        'id': 'dk_table',
        'type': 'raster',
        'source': {
          'type': 'raster',
          'tiles': [
              'mapbox://mapbox2024.6clb3bwa'
/*
            'http://localhost:8080/geoserver/hannan/wms?service=WMS&version=1.1.0&request=GetMap&layers=hannan:dk_table&styles=&bbox={bbox-epsg-3857}&width=256&height=256&srs=EPSG:3857&format=image/png'
*/
          ],
          'tileSize': 256
        },
        'paint': {}
      });
    });



  }
};
</script>

<style>
/* 你可以在这里添加CSS样式 */
</style>
