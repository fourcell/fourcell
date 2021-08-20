<template>
  <div class="mixin-components-container">
    <el-row>
      <el-card class="box-card" style="text-align: left">
        <div slot="header" class="clearfix title">
          <span>Vue框架中嵌入使用wavesurfer.js插件</span>
        </div>
        <div id="waveform" ref="waveform">
          <!-- Here be the waveform -->
        </div>
        <div id="wave-timeline" ref="wave-timeline">
          <!--时间轴 -->
        </div>
        <div>
          <el-button type="primary" @click="playMusic">
            <i class="el-icon-video-play"></i>
            播放 /
            <i class="el-icon-video-pausee"></i>
            暂停
          </el-button>
        </div>
      </el-card>
    </el-row>
  </div>
</template>
<script>
import WaveSurfer from "wavesurfer.js";
import Timeline from "wavesurfer.js/dist/plugin/wavesurfer.timeline.js";
import Regions from "wavesurfer.js/dist/plugin/wavesurfer.regions.js";
export default {
  name: "Details",
  data() {
    return {
      wavesurfer: null,
    };
  },
  mounted() {
    this.$nextTick(() => {
      this.wavesurfer = WaveSurfer.create({
        container: this.$refs.waveform,
        waveColor: "#409EFF",
        progressColor: "blue",
        backend: "MediaElement",
        mediaControls: false,
        audioRate: "1",
        //使用时间轴插件
        plugins: [
          Timeline.create({
            container: "#wave-timeline",
          }),
          Regions.create({
            regions: [
              {
                start: 1,
                end: 3,
                id: 1,
                loop: false,
                color: "hsla(400, 100%, 30%, 0.5)",
                label: "12256",
                text: "123",
                attributes: {
                  value: "小胡加油啊",
                },
              },
              {
                start: 5,
                end: 7,
                id: 2,
                loop: false,
                color: "hsla(200, 50%, 70%, 0.4)",
                minLength: 1,
              },
            ],
            dragSelection: {
              slop: 5,
            },
          }),
        ],
      });

      this.wavesurfer.addRegion({
        start: 8,
        end: 10,
        id: 1,
        loop: false,
        color: "hsla(400, 100%, 30%, 0.5)",
        label: "12256",
        text: "123",
        attributes: {
          value: "小胡加油啊",
        },
      });
      const region_list = document.querySelectorAll(".wavesurfer-region");
      try {
        Object.keys(region_list).map((item) => {
          const elem = document.createElement("span");
          const value = region_list[item].getAttribute("data-region-value");
          if (!value) return;
          elem.innerHTML = value;
          elem.setAttribute("class", "xiaohu");
          region_list[item].appendChild(elem);
        });
      } catch (err) {
        console.log(err);
      }

      // 特别提醒：此处需要使用require(相对路径)，否则会报错
      this.wavesurfer.load(require("../../../assets/test.mp4"));
    });
  },
  methods: {
    playMusic() {
      //"播放/暂停"按钮的单击触发事件，暂停的话单击则播放，正在播放的话单击则暂停播放
      this.wavesurfer.playPause.bind(this.wavesurfer)();
      console.log(this.wavesurfer.regions.list);
    },
  },
};
</script>
<style scoped>
.mixin-components-container {
  background-color: #f0f2f5;
  padding: 30px;
  min-height: calc(100vh - 84px);
}
.region.wavesurfer-region .xiaohu {
  color: red;
}
</style>