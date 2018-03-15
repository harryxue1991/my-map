<template>

    <div id="container">

    </div>

</template>

<script>
const L = require('leaflet');
require('leaflet/dist/leaflet.css');

export default {
  name: 'Map',
  props: {
    msg: String
  },
  methods:{
    tms_init(){
      L.TileLayer.ChinaProvider = L.TileLayer.extend({

          initialize: function(type, options) { // (type, Object)
              var providers = L.TileLayer.ChinaProvider.providers;

              var parts = type.split('.');

              var providerName = parts[0];
              var mapName = parts[1];
              var mapType = parts[2];

              var url = providers[providerName][mapName][mapType];
              options.subdomains = providers[providerName].Subdomains;

              L.TileLayer.prototype.initialize.call(this, url, options);
          }
      });

      L.TileLayer.ChinaProvider.providers = {
          TianDiTu: {
              Normal: {
                  Map: "http://t{s}.tianditu.cn/DataServer?T=vec_w&X={x}&Y={y}&L={z}",
                  Annotion: "http://t{s}.tianditu.cn/DataServer?T=cva_w&X={x}&Y={y}&L={z}",
              },
              Satellite: {
                  Map: "http://t{s}.tianditu.cn/DataServer?T=img_w&X={x}&Y={y}&L={z}",
                  Annotion: "http://t{s}.tianditu.cn/DataServer?T=cia_w&X={x}&Y={y}&L={z}",
              },
              Terrain: {
                  Map: "http://t{s}.tianditu.cn/DataServer?T=ter_w&X={x}&Y={y}&L={z}",
                  Annotion: "http://t{s}.tianditu.cn/DataServer?T=cta_w&X={x}&Y={y}&L={z}",
              },
              Subdomains: ['0', '1', '2', '3', '4', '5', '6', '7']
          },

          GaoDe: {
              Normal: {
                  Map: 'http://webrd0{s}.is.autonavi.com/appmaptile?lang=zh_cn&size=1&scale=1&style=8&x={x}&y={y}&z={z}',
              },
              Satellite: {
                  Map: 'http://webst0{s}.is.autonavi.com/appmaptile?style=6&x={x}&y={y}&z={z}',
                  Annotion: 'http://webst0{s}.is.autonavi.com/appmaptile?style=8&x={x}&y={y}&z={z}'
              },
              Subdomains: ["1", "2", "3", "4"]
          },

          Google: {
              Normal: {
                  Map: "http://www.google.cn/maps/vt?lyrs=m@189&gl=cn&x={x}&y={y}&z={z}"
              },
              Satellite: {
                  Map: "http://www.google.cn/maps/vt?lyrs=s@189&gl=cn&x={x}&y={y}&z={z}"
              },
              Subdomains: []
          },

          Geoq: {
              Normal: {
                  Map: "http://map.geoq.cn/ArcGIS/rest/services/ChinaOnlineCommunity/MapServer/tile/{z}/{y}/{x}",
                  Color: "http://map.geoq.cn/ArcGIS/rest/services/ChinaOnlineStreetColor/MapServer/tile/{z}/{y}/{x}",
                  PurplishBlue: "http://map.geoq.cn/ArcGIS/rest/services/ChinaOnlineStreetPurplishBlue/MapServer/tile/{z}/{y}/{x}",
                  Gray: "http://map.geoq.cn/ArcGIS/rest/services/ChinaOnlineStreetGray/MapServer/tile/{z}/{y}/{x}",
                  Warm: "http://map.geoq.cn/ArcGIS/rest/services/ChinaOnlineStreetWarm/MapServer/tile/{z}/{y}/{x}",
                  Cold: "http://map.geoq.cn/ArcGIS/rest/services/ChinaOnlineStreetCold/MapServer/tile/{z}/{y}/{x}"
              },
              Subdomains: []

          }
      };

      L.tileLayer.chinaProvider = function(type, options) {
          return new L.TileLayer.ChinaProvider(type, options);
      };
    },
    map_init(){
      var normalm = L.tileLayer.chinaProvider('TianDiTu.Normal.Map', {
              maxZoom: 18,
              minZoom: 5
          }),
          normala = L.tileLayer.chinaProvider('TianDiTu.Normal.Annotion', {
              maxZoom: 18,
              minZoom: 5
          }),
          imgm = L.tileLayer.chinaProvider('TianDiTu.Satellite.Map', {
              maxZoom: 18,
              minZoom: 5
          }),
          imga = L.tileLayer.chinaProvider('TianDiTu.Satellite.Annotion', {
              maxZoom: 18,
              minZoom: 5
          });
      var normal = L.layerGroup([normalm, normala]),
          image = L.layerGroup([imgm, imga]);
      var baseLayers = {
          "地图": normal,
          "影像": image,
      }
      var overlayLayers = {
      }
      var map = L.map("container", {
          center: [31.59, 120.29],
          zoom: 12,
          layers: [normal],
          zoomControl: false
      });
      L.control.layers(baseLayers, overlayLayers).addTo(map);
      L.control.zoom({
          zoomInTitle: '放大',
          zoomOutTitle: '缩小'
      }).addTo(map);
      this.map = map;
    }
  },
  mounted(){
    this.tms_init();
    this.map_init();
    let aLayer = L.layerGroup();
    //let bLayer = L.layerGroup();

    let m = L.marker([31.59, 120.29]);
    m.addTo(aLayer);
    aLayer.addTo(this.map);
    
    // m.addTo(this.map);
    
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#container{
    height:100%;
    width:100%;
}
</style>
