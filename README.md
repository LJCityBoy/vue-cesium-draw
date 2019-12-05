### Cesium-Draw
This is a Vue+Cesium project, which provides functions such as drawing and editing graphics as well as importing and exporting them.
Key Features ：
- Add markers, you can extend the markers style, edit and delete them.
- Interactive drawing of polyline and polygon openings allows editing and setting of nodes and their styles.
- Export and import.（json,Geojson,shp）
#### Blog
[https://blog.csdn.net/xtfge0915/article/details/102483418](https://blog.csdn.net/xtfge0915/article/details/102483418)

#### Install
```
npm install cesium-draw --save
```
#### Usage
```js
//main.js
import Vue from 'vue'
import cesiumDrawHandler from 'cesium-draw'
Vue.use(cesiumDrawHandler)
```
```js
//you-component.js
<templete>
<div>
<div id='cesiumContainer'></div>
<cesiumDrawViewer :viewer="viewer" v-if="mounted"></cesiumDrawViewer>
</div>
</templete>
<script>
export default {
  name: "my-component",
  data(){

  },
  mounted(){
      this.viewer=new Cesium.Viewer('cesiumContainer')
  }

}
</script>
```
#### Development
```
npm install
npm start
```
#### Build
```
npm run build
```

### 效果
![avatar](https://img-blog.csdnimg.cn/20190524155136375.gif)
![avatar](https://img-blog.csdnimg.cn/20190524155207442.gif)
