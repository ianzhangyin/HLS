<template>
  <demo-section>
    <demo-block title="基础用法">
      <video-player
        style="width: 100%; max-width: 600px"
        :options="videoOptions"
        @ready="
          (player) =>
            player.src({
              src: 'https://static.refined-x.com/static/1080p-watermark.mp4?t=media111',
              type: 'video/mp4',
            })
        "
      />
    </demo-block>
    <demo-block title="禁止拖拽">
      <video-player
        style="width: 100%; max-width: 600px"
        :options="videoOptions"
        :seek-able="false"
        @ready="
          (player) =>
            player.src({
              src: 'https://static.refined-x.com/static/1080p-watermark.mp4',
              type: 'video/mp4',
            })
        "
      />
    </demo-block>
    <demo-block title="视频打点">
      <video-player
        style="width: 100%; max-width: 600px"
        :options="videoOptions"
        @ready="handleReadyMark"
      />
    </demo-block>
    <demo-block title="清晰度选择">
      <video-player
        style="width: 100%; max-width: 600px"
        :options="videoOptions"
        @ready="handleReadySwitch"
      />
    </demo-block>
    <demo-block title="HLS加密">
      <video-player
        ref="vpl"
        style="width: 100%; max-width: 600px"
        :options="{
          ...videoOptions,
          CryptoConfig: {
            ...CryptoConfig,
          },
          RequestOption: {
            headers: {
              Authorization: 'aa98187bab55403393f8cd01d0229b59',
            },
          },
        }"
        :watermark-config="{
          wmText: 'test123',
          color: '#fff',
          position: 5,
          opacity: 0.5,
        }"
        @ready="handleReadyHLS"
      />
    </demo-block>
  </demo-section>
</template>

<script>
// import * as util from "@/main/assets/util";

export default {
  data() {
    return {
      videoOptions: {
        // autoplay: true,
        controls: true,
      },
      CryptoConfig: {
        Enable: false,
        Debug: true,
        async GetSecretKey() {
          // 支持异步
          return new Promise((resolve) => {
            setTimeout(() => {
              resolve(12345);
            }, 1000);
          });
        },
      },
    };
  },
  methods: {
    handleReadyMark(player) {
      player.src({
        src: 'https://static.refined-x.com/static/1080p-watermark.mp4',
        type: 'video/mp4',
      });

      player.markerPlugin({
        //  打点信息
        markers: [
          {
            offset: 10,
          },
          {
            offset: 20,
            data: {
              content: '可点击',
            },
            onClick(e) {
              e.stopPropagation(); // marker点击事件，可以屏蔽原进度条动作
              alert('marker click!');
            },
          },
        ],
      });
    },
    handleReadySwitch(player) {
      // 配置插件
      player.SwitcherPlugin();

      // 动态配置视频源
      player.updateSrc([
        {
          src: 'https://static.refined-x.com/static/1080p-watermark.mp4',
          type: 'video/mp4',
          label: '1080P',
          res: '1080',
        },
        {
          src: 'https://static.refined-x.com/stream/1080p-720.m3u8',
          type: 'application/x-mpegURL',
          label: '720P',
          res: '720',
        },
      ]);
    },
    handleReadyHLS(player) {
      player.src({
        src: 'https://static.refined-x.com/static/hls/hls-demo.m3u8', // 七牛云demo
        // src: 'http://medicalshare.kaifa/oss/2022/06/14/38/6bc30f1e49e54fb4b579f69ab60259cf/902ecd8a-a696-49fd-9a9f-74d3a99a2c69.m3u8',
        type: 'application/x-mpegURL',
      });
    },
  },
};
</script>
