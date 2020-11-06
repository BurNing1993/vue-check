<template>
  <div id="map"></div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from "vue";
import AMapLoader from "@amap/amap-jsapi-loader";

export default defineComponent({
  name: "CheckMap",
  emits: ["change"],
  setup(_, { emit }) {
    let AMap;
    let geolocation;

    const center = [104.06901, 30.56975];
    let centerPoint;
    const maxDistance = 1000;

    let inCircle = ref(false);
    let currentPos = ref([]);
    let currentDistance = ref(0);

    const getLocation = () => {
      geolocation.getCurrentPosition(function (status, result) {
        if (status == "complete") {
          const lng = result.position.lng;
          const lat = result.position.lat;
          const p = new AMap.LngLat(lng, lat);
          currentPos.value = [lng, lat];
          const distance = Math.round(centerPoint.distance(p));
          currentDistance.value = distance;
          inCircle.value = distance <= maxDistance;
          emit("change", inCircle);
        } else {
          console.error(status, result);
        }
      });
    };

    const initMap = async () => {
      try {
        AMap = await AMapLoader.load({
          key: "3046f322a03cb1445c16424dcc230cbe", // 申请好的Web端开发者Key，首次调用 load 时必填
          version: "2.0", // 指定要加载的 JSAPI 的版本，缺省时默认为 1.4.15
          plugins: ["AMap.Scale", "AMap.Geolocation"], // 需要使用的的插件列表，如比例尺'AMap.Scale'等
        });
        let map = new AMap.Map("map", {
          resizeEnable: true,
          zoom: 12,
          center,
        });
        // 画圈
        centerPoint = new AMap.LngLat(center[0], center[1]);
        const circle = new AMap.Circle({
          center: centerPoint, // 圆心位置
          radius: maxDistance, //半径
          strokeColor: "#F33", //线颜色
          strokeOpacity: 1, //线透明度
          strokeWeight: 3, //线粗细度
          fillColor: "#ee2200", //填充颜色
          fillOpacity: 0.35, //填充透明度
        });
        map.add(circle);
        map.setFitView();

        //定位
        AMap.plugin("AMap.Geolocation", function () {
          geolocation = new AMap.Geolocation({
            enableHighAccuracy: true, //是否使用高精度定位，默认:true
            timeout: 10000, //超过10秒后停止定位，默认：5s
            position: "RB", //定位按钮的停靠位置
            buttonOffset: new AMap.Pixel(10, 20), //定位按钮与设置的停靠位置的偏移量，默认：Pixel(10, 20)
            zoomToAccuracy: true, //定位成功后是否自动调整地图视野到定位点
            showCircle: false, //是否显示定位精度圆，默认为true
            markerOptions: {
              //自定义定位点样式，同Marker的Options
              offset: new AMap.Pixel(-18, -36),
              content:
                '<img src="https://a.amap.com/jsapi_demos/static/resource/img/user.png" style="width:36px;height:36px"/>',
            },
          });
          map.addControl(geolocation);
          getLocation();
        });
      } catch (error) {
        console.error(error);
      }
    };
    onMounted(initMap);
    return {
      getLocation,
      inCircle,
      currentPos,
      currentDistance,
    };
  },
});
</script>


<style scoped>
#map {
  height: 100%;
}
</style>