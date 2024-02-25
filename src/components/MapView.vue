<template>
  <div id="map" class="map"></div>
</template>

<script>
import Map from 'ol/Map';
import View from 'ol/View';
import TileLayer from 'ol/layer/Tile';
import TileWMS from 'ol/source/TileWMS';

export default {
  name: 'MapView',
  mounted() {
    const layers = [
      'npk_table',
      'xzq_table',
      'dzg_table',
      'dgg_table',
      'dgz_table',
      'dzg_table',
      'dzz_table',
      'ggg_table',
      'ggz_table',
      'zdz_table',
      'zgg_table',
      'zgz_table',
      'zzz_table',
      'xzq_table',
      'dk_table',
      'point_table'
      // 添加更多的图层名称...
    ].map(layerName => new TileLayer({
      source: new TileWMS({
        url: 'http://localhost:8080/geoserver/hannan/wms',
        params: {
          'LAYERS': `hannan:${layerName}`,
          'STYLES': `${layerName}`, // geoserver的style中自定义颜色,与表同名
          'TILED': true,
          'VERSION': '1.1.0',
          'FORMAT': 'image/png',
          'SRS': 'EPSG:4326', // 根据需要选择'EPSG:4326'或'EPSG:3857'
        },
        serverType: 'geoserver',
      }),
    }));

/*    layers[0].setZIndex(100); // 假设layers[8]是point_table图层，设置一个高Z索引确保它在最顶层
    layers[0].getSource().updateParams({
      'STYLES': 'your_bubble_style' // 'your_bubble_style'是在GeoServer中定义的气泡样式名称
    });*/

    new Map({
      target: 'map',
      layers: layers,
      view: new View({
        projection: 'EPSG:4326', // 确保视图的投影与WMS服务的投影相匹配
        center: [113.92793273925781, 30.27973175048828], // 这是bbox中心点的大概坐标
        zoom: 12 // 选择适当的缩放级别
      })
    });
  }
};
</script>

<style>
.map {
  height: 400px;
  width: 100%;
}
</style>

