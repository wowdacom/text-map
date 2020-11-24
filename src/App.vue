<template>
  <div id="app">
    <div id="mapid"></div>
  </div>
</template>

<script>
import L from "leaflet";
import "leaflet-draw";
import "leaflet-draw/dist/leaflet.draw.css";

export default {
  name: "App",
  components: {},
  data() {
    return {
      map: null,
    };
  },
  mounted() {
    let _self = this;
    this.map = L.map("mapid").setView([25.0299114, 121.549561], 25);

    delete L.Icon.Default.prototype._getIconUrl;

    L.Icon.Default.mergeOptions({
      iconRetinaUrl: require("leaflet/dist/images/marker-icon-2x.png"),
      iconUrl: require("leaflet/dist/images/marker-icon.png"),
      shadowUrl: require("leaflet/dist/images/marker-shadow.png"),
    });

    //好粗，很多細節的東西看不到

    L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
      attribution:
        '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
      maxZoom: 19,
    }).addTo(_self.map);

    L.marker([25.0299114, 121.549561])
      .addTo(_self.map)
      .bindPopup("好房網")
      .openPopup();

    L.marker([25.0307116, 121.5499475])
      .addTo(_self.map)
      .bindPopup("豆漿大王")
      .openPopup();

    var triangle = [
      [25.031323, 121.529555],
      [25.032237, 121.529764],
      [25.031459, 121.530027],
    ];
    L.polygon(triangle, {
      color: "#000eff",
      fillColor: "#0000ed",
      weight: 1,
    }).addTo(_self.map);

    var drawnItems = new L.FeatureGroup();
    _self.map.addLayer(drawnItems);
    var drawControl = new L.Control.Draw({
      edit: {
        featureGroup: drawnItems,
      },
    });
    _self.map.addControl(drawControl);
    // L.circle([25.0306284,121.5288324],{color:'#FF0000',fillColor:'#FF0000',radius:10,fillOpacity:1}).addTo(map)

    // var latlngs =[[25.0306284,121.5278324],[25.0306284,121.5288324],[25.0306284,121.5298324]]
    // L.polyline(latlngs, {color: 'red'}).addTo(map);

    // var latlngs2 = [[25.0296284,121.5298324],[25.0316284,121.5298324], [25.0316284,121.5278324], [25.0296284,121.5278324]]
    // var polygon = L.polygon(latlngs2, {color: '#000eff',fillColor:'#0000ed',
    //               weight:1}).addTo(map);

    // L.popup()
    //         .setLatLng([25.0306284,121.5288324])
    //         .setContent('哈囉你好嗎?')
    //         .openOn(map);

    // polygon.bindPopup("我是個多邊形").openPopup();

    L.control
      .zoom({ zoomInTitle: "放大吧", zoomOutTitle: "缩小吧" })
      .addTo(_self.map);

    //多了之後可以寫成這樣
    // var baseLayers = {
    // "Mapbox": mapbox,
    // "OpenStreetMap": osm
    // };
    // var overlays = {
    //     "Marker": marker,
    //     "Roads": roadsLayer
    // };
    // L.control.layers(baseLayers, overlays).addTo(map);

    // create popup contents
    var customPopup = function (latlng) {
      return `<div class="popup-box">
                  <h1>新增地點</h1>
                  <p>緯度：${latlng.lat}</p>
                  <p>經度：${latlng.lng}</p>
                  <input class="popup-input" placeholder='新增店家'></input>
                  <div style="margin-top: 10px;display: flex; justify-content: space-around;">
                    <button data-latlng="[${latlng.lat},${latlng.lng}]" class="popup-add">新增</button><button class="popup-cancer">取消</button>
                  </div>
              </div>`;
    };

    // document.querySelector("#mapid").addEventListener("click", function(){
    //   if(event.target.className === "popup-add") {
    //     let input = document.querySelector(".popup-input");
    //     L.marker(JSON.parse(event.target.dataset.latlng))
    //         .addTo(_self.map)
    //         .bindPopup(input.value)
    //         .openPopup();
    //     _self.map.closePopup();
    //   } else if (event.target.className === "popup-cancer") {
    //     _self.map.closePopup();
    //   }

    // })

    // specify popup options
    var customOptions = {
      maxWidth: "500",
      className: "custom",
    };

    //找經緯度有用的工具
    _self.map.on("click", function (e) {

      console.log(e)
      L.popup()
        .setLatLng(e.latlng)
        .setContent(customPopup(e.latlng), customOptions)
        .openOn(_self.map)
        
    });

    _self.map.on("dblclick", function (e) {
      L.marker(e.latlng).addTo(_self.map);
    });
  },
  methods: {
    addMarks() {
      console.log("hello");
    },
  },
};
</script>

<style lang="scss">
@import url("../node_modules/leaflet/dist/leaflet.css");

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  #mapid {
    margin: auto 0;
    height: 100vh;
    width: 100vw;
    button {
        cursor: pointer;
    }
  }
}
</style>
