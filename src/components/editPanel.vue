<!--
@Author:zhangbo
@Date:2019-05-21 19:36:13
@E-mail:zhangb@geovie.com.cn
-->
<template>
  <div class="graphic-edit-panel" :id="graphic.name" v-show="isShow">
    <el-container v-if="graphic.geomType==='polyline'">
      <el-header>
        <span class="el-icon-user-solid" style="margin-left: -20px" @click="showMenu=!showMenu"></span>
        <div class="show-drop-down" v-show="showMenu">
          <div
            class="drop-down-item"
            v-for="(item, index) in [{'value':'导出','key':'showExport'},
               {'value':'删除','key':'remove'},
               {'value':'属性','key':'attribute'}]"
            :key="index"
            @click="command(item.key)"
          >{{item.value}}</div>
        </div>
        <div>{{title}}</div>
        <span class="closebtn" @click="closeHandler"></span>
      </el-header>
      <el-main class="geom-line">
        <div>
          <span class="p-class">颜色：</span>
          <el-color-picker v-model="color" size="mini"></el-color-picker>
          <span class="p-class">顶点：</span>
          <el-switch v-model="node" size="mini" active-color="#13ce66" inactive-color="#ff0000"></el-switch>
        </div>
        <div>
          <span class="p-class">线宽：</span>
          <el-slider v-model="Width" :step="1" :max="10" :min="0"></el-slider>
          <el-input size="mini" v-model="width"></el-input>
        </div>
      </el-main>
    </el-container>
    <el-container v-if="graphic.geomType==='polygon'">
      <el-header>
        <span class="el-icon-user-solid" style="margin-left: -20px" @click="showMenu=!showMenu"></span>
        <div class="show-drop-down" v-show="showMenu">
          <div
            class="drop-down-item"
            v-for="(item, index) in [{'value':'导出','key':'export'},
               {'value':'删除','key':'remove'},
               {'value':'属性','key':'attribute'}]"
            :key="index"
            @click="command(item.key)"
          >{{item.value}}</div>
        </div>
        <div>{{title}}</div>
        <span class="closebtn" @click="closeHandler"></span>
      </el-header>
      <el-main :class="{'geom-polygon':outline,'geom-line':!outline}">
        <div>
          <span class="p-class">颜色：</span>
          <el-color-picker v-model="color" size="mini"></el-color-picker>
          <span class="p-class">顶点：</span>
          <el-switch v-model="node" size="mini" active-color="#13ce66" inactive-color="#ff0000"></el-switch>
        </div>
        <div style="margin-bottom: 5px">
          <span class="p-class">边框：</span>
          <el-switch v-model="outline" size="mini" active-color="#13ce66" inactive-color="#ff0000"></el-switch>
        </div>
        <div v-if="outline">
          <span class="p-class">颜色：</span>
          <el-color-picker v-model="outlineColor" size="mini"></el-color-picker>
        </div>
        <div v-if="outline">
          <span class="p-class">线宽：</span>
          <el-slider v-model="outWidth" :step="1" :max="10" :min="0"></el-slider>
          <el-input size="mini" v-model="outlineWidth"></el-input>
        </div>
      </el-main>
    </el-container>
    <attributeViewer ref="attributeTable"></attributeViewer>
    <el-dialog :visible.sync="exportdiag" width="30%" :modal-append-to-body="false">
      请输入文件名：
      <el-input v-model="exportName"></el-input>
      <span slot="footer" class="dialog-footer">
        <el-button @click="exportdiag = false">取 消</el-button>
        <el-button type="primary" @click="exportdiag=false;command('export')">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import attributeViewer from "./attribute";
export default {
  data() {
    return {
      isShow: true,
      exportdiag: false,
      exportName:"",
      title: "要素编辑",
      color: "#FF0000",
      width: 3,
      node: true,
      graphic: {},
      outline: false,
      outlineWidth: 1,
      outlineColor: "#FF0000",
      showMenu: false
    };
  },
  props: {},
  components: { attributeViewer },
  computed: {
    Width: {
      get() {
        return parseInt(this.width);
      },
      set(v) {
        this.width = v;
      }
    },
    outWidth: {
      get() {
        return parseFloat(this.outlineWidth);
      },
      set(v) {
        this.outlineWidth = v;
      }
    }
  },
  mounted() {
    // const self = this
  },
  methods: {
    command(command) {
      if (command === "attribute") {
        this.$refs.attributeTable.open();
      } else if (command === "showExport") {
        this.exportdiag=true
      } else {
        this.graphic[command](this.exportName);
      }

      this.showMenu = false;
    },
    displayTrigger(v) {
      this.isShow = v;
    },
    closeHandler() {
      this.isShow = false;
      this.graphic.stopEdit();
    }
  },
  watch: {
    color(n, o) {
      if (n === o) {
        return;
      }
      this.graphic.setColor(n);
    },
    width(n, o) {
      if (n === "") {
        this.width = 0;
        return;
      }
      if (n === o) {
        return;
      }
      this.graphic.setWidth(n);
    },
    node(n, o) {
      // this.graphic.setNode(n)
      if (n === o) {
        return;
      }
      this.graphic.setNodeAction = n;
    },
    outline(n, o) {
      if (n === o) return;
      this.graphic.setOutline(n);
    },
    outlineWidth(n, o) {
      if (n === "" && n !== o) {
        this.outlineWidth = 0;
        return;
      }
      this.graphic.setOutlineWidth(n);
    },
    outlineColor(v) {
      this.graphic.setOutlineColor(v);
    }
  }
};
</script>

<style lang="scss" scoped>
.graphic-edit-panel {
  z-index: 999;
}
@import "../assets/css/globeStyle";
</style>
