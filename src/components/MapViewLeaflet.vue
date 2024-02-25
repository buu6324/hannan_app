<template>
  <div>
    <div id="map" class="map"></div>
  </div>
</template>

<script>
import L from 'leaflet';
import 'leaflet/dist/leaflet.css';
import customIconUrl from '@/assets/icon.png';
import geojsonData from '@/assets/point_table.json'; // 引入 GeoJSON 文件
import logoUrl from '@/assets/hannan_logo.png'; // 引入Logo文件
import signatureLogoUrl from '@/assets/signature_logo.png'; // 引入签名文件


export default {
  name: 'MapViewLeaflet',
  mounted() {
    const map = L.map('map', {
      minZoom: 12, // 最小缩放级别
      maxZoom: 14, // 最大缩放级别
    }).setView([30.27973175048828, 113.92793273925781], 12);

    // 加载图层组
    const wmsLayer = L.tileLayer.wms("http://localhost:8080/geoserver/hannan/wms", {
      layers: 'hannan:hannan',
      format: 'image/png',
      version: '1.1.0',
      transparent: true,
      tileSize: 512,
      srs: 'EPSG:4326',
    }).addTo(map);

    // 自定义图标
    const customIcon = L.icon({
      iconUrl: customIconUrl,
      iconSize: [20, 20],
      iconAnchor: [13, 13],
    });

    // 添加GeoJSON图层
    L.geoJSON(geojsonData, {
      onEachFeature: function(feature, layer) {
        if (feature.properties) {
          let popupContent = '<p>';
          for (let prop in feature.properties) {
            popupContent += prop + ': ' + feature.properties[prop] + '<br>';
          }
          popupContent += '</p>';
          layer.bindPopup(popupContent);
        }
      },
      pointToLayer: function(feature, latlng) {
        return L.marker(latlng, {icon: customIcon});
      }
    }).addTo(map);

    // 添加自定义控件以展示图层样式信息
    const styleInfoControl = L.control({position: 'topright'});
    styleInfoControl.onAdd = function(map) {
      var div = L.DomUtil.create('div', 'info legend');
      // 这里添加图层样式的描述或信息
      div.innerHTML = "<div style=\"background-color: #0047ff; padding: 15px;\">\n" +
          "    <span style=\"color: white; font-weight: bold;\">低</span>\n" +
          "    <span style=\"color: white; font-weight: bold;\">高</span>\n" +
          "    <span style=\"color: white; font-weight: bold;\">高</span>\n" +
          "</div>\n" +
          "\n" +
          "<div style=\"background-color: #134f5c; padding: 15px;\">\n" +
          "    <span style=\"color: white; font-weight: bold;\">中</span>\n" +
          "    <span style=\"color: white; font-weight: bold;\">中</span>\n" +
          "    <span style=\"color: white; font-weight: bold;\">中</span>\n" +
          "</div>\n" +
          "\n" +
          "<div style=\"background-color: #999999; padding: 15px;\">\n" +
          "    <span style=\"color: white; font-weight: bold;\">中</span>\n" +
          "    <span style=\"color: white; font-weight: bold;\">中</span>\n" +
          "    <span style=\"color: white; font-weight: bold;\">高</span>\n" +
          "</div>\n" +
          "\n" +
          "<div style=\"background-color: #9900ff; padding: 15px;\">\n" +
          "    <span style=\"color: white; font-weight: bold;\">中</span>\n" +
          "    <span style=\"color: white; font-weight: bold;\">高</span>\n" +
          "    <span style=\"color: white; font-weight: bold;\">中</span>\n" +
          "</div>\n" +
          "\n" +
          "<div style=\"background-color: #00ff00; padding: 15px;\">\n" +
          "    <span style=\"color: white; font-weight: bold;\">中</span>\n" +
          "    <span style=\"color: white; font-weight: bold;\">高</span>\n" +
          "    <span style=\"color: white; font-weight: bold;\">高</span>\n" +
          "</div>\n" +
          "\n" +
          "<div style=\"background-color: #ead1dc; padding: 15px;\">\n" +
          "    <span style=\"color: white; font-weight: bold;\">中</span>\n" +
          "    <span style=\"color: white; font-weight: bold;\">低</span>\n" +
          "    <span style=\"color: white; font-weight: bold;\">中</span>\n" +
          "</div>\n" +
          "\n" +
          "<div style=\"background-color: #783f04; padding: 15px;\">\n" +
          "    <span style=\"color: white; font-weight: bold;\">高</span>\n" +
          "    <span style=\"color: white; font-weight: bold;\">高</span>\n" +
          "    <span style=\"color: white; font-weight: bold;\">中</span>\n" +
          "</div>\n" +
          "\n" +
          "<div style=\"background-color: #ffd966; padding: 15px;\">\n" +
          "    <span style=\"color: white; font-weight: bold;\">高</span>\n" +
          "    <span style=\"color: white; font-weight: bold;\">高</span>\n" +
          "    <span style=\"color: white; font-weight: bold;\">高</span>\n" +
          "</div>\n" +
          "\n" +
          "<div style=\"background-color: #ffff00; padding: 15px;\">\n" +
          "    <span style=\"color: black; font-weight: bold;\">低</span>\n" +
          "    <span style=\"color: black; font-weight: bold;\">中</span>\n" +
          "    <span style=\"color: black; font-weight: bold;\">中</span>\n" +
          "</div>\n" +
          "<div style=\"background-color: #00ffff; padding: 15px;\">\n" +
          "  <span style=\"color: white; font-weight: bold;\">低</span>\n" +
          "  <span style=\"color: white; font-weight: bold;\">高</span>\n" +
          "  <span style=\"color: white; font-weight: bold;\">中</span>\n" +
          "</div>"
      return div;
    };
    styleInfoControl.addTo(map);

    //添加logo
    const logoControl = L.control();
    logoControl.onAdd = function(map) {
      const img = L.DomUtil.create('img', 'logo-class');
      img.src = logoUrl;
      img.style.width = '300px'; // 设置Logo的尺寸，根据需要调整
      img.style.margin = '0px'; // 设置Logo的外边距，根据需要调整
      return img;
    };
    logoControl.addTo(map);

    //添加签名
    const signature = L.control({position: 'bottomright'});
    signature.onAdd = function(map) {
      const img = L.DomUtil.create('img');
      img.src = signatureLogoUrl;
      img.style.width = '150px'; // 设置Logo的尺寸，根据需要调整
      img.style.margin = '0px'; // 设置Logo的外边距，根据需要调整
      return img;
    };
    signature.addTo(map);

    // 处理地图点击事件，获取特征信息
    map.on('click', function(e) {
      var url = getFeatureInfoUrl(
          map,
          wmsLayer,
          e.latlng,
          {
            'info_format': 'text/html' // 或 'application/json'
          }
      );

      fetch(url)
          .then(function(response) { return response.text(); })
          .then(function(html) {
            L.popup({ maxWidth: 800 })
                .setLatLng(e.latlng)
                .setContent(html)
                .openOn(map);
          });
    });
  }
};
</script>

<style>
.map {
  position: fixed;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100vw;
}

.logo-class {
  position: absolute;
  top: -100px;  /* 调整这个值来向下移动Logo */
  right: 1200px; /* 调整这个值来向右移动Logo */
  z-index: 1000; /* 确保Logo在地图控件之上 */
  pointer-events: none; /* 可选：防止Logo拦截鼠标事件 */
}
</style>
