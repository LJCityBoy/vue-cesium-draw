<!--
@Author:zhangbo
@Date:2019-03-13 10:36:32
@E-mail:zhangb@geovie.com.cn
@Last Modified by:zhangbo
@Modify-Date:2019-9-24 20:07

-->
<template>
  <div style="width:100%;height: 100%" class="fullSize">
    <div class="full-container" :style="viewStyle" id="cesiumContainer"></div>
    <div id="loadingOverlay">
      <h1>Loading...</h1>
    </div>
  </div>
</template>

<script>
export default {
  name: "EarthViewer",
  viewerProperty: {},
  props: {
    viewStyle: {},
    viewerProperty: {},
    dropBackground: {
      default: false
    }
  },
  components: {},
  data() {
    return {
      viewer: null
    };
  },
  computed: {},
  mounted() {
    const Bus=window.Bus
    const Cesium=window.Cesium
    const _this = this;
    _this.viewerDefaultProperty = {
      animation: false,
      timeline: false,
      geocoder: false,
      homeButton: false,
      navigationHelpButton: false,
      baseLayerPicker: false,
      fullscreenElement: "cesiumContainer",
      fullscreenButton: false,
      shouldAnimate: true,
      infoBox: false,
      selectionIndicator: false,
      sceneModePicker: false,
      shadows: false,
      //去黑色背景
      skyAtmosphere: false,
      imageryProvider: new Cesium.UrlTemplateImageryProvider({
        url: "http://www.google.cn/maps/vt?lyrs=s@800&x={x}&y={y}&z={z}",
        tilingScheme: new Cesium.WebMercatorTilingScheme(),
        minimumLevel: 1,
        maximumLevel: 20
      })
    };

    //设置Veiwer属性
    for (let property in _this.viewerProperty) {
      _this.viewerDefaultProperty[property] = _this.viewerProperty[property];
    }

    const viewer = new Cesium.Viewer(
      "cesiumContainer",
      _this.viewerDefaultProperty
    );
    window.viewer = viewer;
    if (Cesium.defined(viewer)) {
      Bus.$emit("viewerMounted");
    }

    //清除cesium-widget-credits
    const credits = document.getElementsByClassName("cesium-widget-credits");
    credits[0].parentElement.removeChild(credits[0]);

    //禁止双击zoom
    viewer.cesiumWidget.screenSpaceEventHandler.removeInputAction(
      Cesium.ScreenSpaceEventType.LEFT_DOUBLE_CLICK
    );
    viewer.camera.flyTo({
      destination:Cesium.Cartesian3.fromDegrees(116.4,39.9,15000000),
      duration:0
    })
  
  },
  methods: {}
};
</script>

<style scoped>
.fullSize,
.full-container {
  position: absolute;
  /*top: 0;*/
  /*left: 0;*/
  border: none;
  height: 100%;
  width: 100%;
  margin: 0px;
  display: inherit;
}
.doubleViewer {
  width: 50%;
}
/*#cesiumContainer {*/
/*overflow: hidden;*/
/*position: fixed;*/
/*background: url('../../static/images/sky.jpg') no-repeat;*/
/*margin: 0px;*/
/*background-size: cover;*/
/*}*/

#loadingOverlay {
  position: absolute;
  top: 0;
  left: 0;
  opacity: 0.9;
  width: 100%;
  height: 100%;
  display: none;
}

#loadingOverlay h1 {
  text-align: center;
  position: relative;
  top: 50%;
  margin-top: -0.5em;
}

#mousePositionId {
  position: absolute;
  right: 30px;
  bottom: 50px;
  z-index: 100;
  font-size: 20px;
}
.layer-picker-class {
  float: right;
}
</style>
<style>
html {
  overflow-x: hidden;
  overflow-y: hidden;
}
</style>
